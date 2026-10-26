# Staged uploads

[Back to the list of all defined endpoints](endpoints.md)

Large files may be staged in bounded, sequential requests before another endpoint consumes them,
without TUS or a separate upload server. Staging creates nothing but the staged file: the resource
that eventually uses it (an import process, a workspace item, ...) is created by that resource's own
endpoint, which then applies its own authorization rules and returns its usual representation.
Currently [script invocation](scripts-endpoint.md#script-invocation-from-staged-uploads) consumes
staged uploads; the multipart forms of the consuming endpoints remain supported.

All endpoints require an authenticated user. Each upload is bound to its creator; anyone else,
including administrators, receives `404` for it. Every request is authenticated on its own with the
normal JWT, and mutating requests also require the usual [CSRF protection](csrf-tokens.md). JWTs and
CSRF tokens are separate mechanisms; neither is replaced by the upload identifier.

## Discovery and preflight

The API root links to the resource:

```json
{
  "_links": {
    "uploads": { "href": "/api/core/uploads" }
  }
}
```

**POST /api/core/uploads**

Content type: `application/json`. Send only metadata, not the file body:

```json
{ "name": "batch.zip", "size": 10485760 }
```

`size` is the exact complete file length in bytes. It must be positive and no larger than the
existing `spring.servlet.multipart.max-file-size` setting (DSpace default: `512MB`). A configured
unlimited value remains unlimited. The limit applies to the *whole file*, not to each chunk.
The filename must be nonblank, at most 255 characters, and contain no `..`, slash, backslash or
control characters. Consuming endpoints may apply stricter rules of their own when they use the file.

The server validates the size **before reserving staging or receiving file bytes**. It returns
`413 Payload Too Large` for an oversized file. Clients must wait for a successful preflight before
sending any chunk, and show a file-size error immediately when rejected.

Successful creation returns `201 Created`, a `Location` header and this HAL representation:

```json
{
  "id": "18c54117-a230-4c10-b6fc-a906b592ddaf",
  "name": "batch.zip",
  "size": 10485760,
  "chunkSize": 8388608,
  "receivedBytes": 0,
  "state": "UPLOADING",
  "result": null,
  "_links": {
    "self": { "href": "/api/core/uploads/18c54117-a230-4c10-b6fc-a906b592ddaf" },
    "chunks": {
      "href": "/api/core/uploads/18c54117-a230-4c10-b6fc-a906b592ddaf/chunks/{offset}",
      "templated": true
    }
  }
}
```

The client must use the returned `chunkSize`, not assume the example value. It is the configured
`staged-upload.chunk-size` (default 8 MiB), capped by a finite `spring.servlet.multipart.max-request-size`.
Chunk requests are not multipart requests; the staging implementation enforces these limits itself.

Creation can also return `400` for invalid metadata, `429` when the user's quota of active uploads
(`staged-upload.max-active-per-user`, default 8) is exhausted, or `409` when staging is briefly locked.
Creation is not idempotent: do not automatically repeat it after an uncertain response. An orphaned
reservation is cleaned up after the retention period.

## Read committed progress

**GET /api/core/uploads/<:uuid>**

Returns `200` with the representation above. `receivedBytes` counts only fully committed chunks,
not an in-flight or partial chunk. Once a consuming endpoint has recorded what it created, `result`
holds that resource's link and a `result` link is added. Missing, cancelled, cleaned-up or
differently owned uploads return `404`.

## Upload a chunk

**PUT /api/core/uploads/<:uuid>/chunks/<:offset>**

Content type: `application/octet-stream`. The body contains exactly the next
`min(chunkSize, size - offset)` bytes, without multipart encoding. The offset starts at zero,
is a multiple of `chunkSize`, and may not skip committed data. Send only one request at a time.

Returns `200` with updated committed progress. A repeated PUT at an already committed offset
is idempotent **only when its bytes are identical**. A short body returns `400`, an oversized
body `413`, and a changed retry, invalid offset, concurrent write or an upload that is no longer
`UPLOADING` returns `409`. Partial data never advances committed progress.

Each request is authenticated independently, so a JWT renewed between chunks is used by the next
chunk. A chunk can still fail on token expiry or a network error; retry only that chunk, or GET the
upload to recover its committed offset. Unauthorized users cannot enter the body-reading controller
method. An upstream proxy may nevertheless buffer a request before forwarding it; bounded chunks
limit the amount at risk and do not override deployment-specific proxy behavior.

## Consume an upload

A staged upload is consumed by the endpoint of the resource it is meant for, using a JSON body
that references the upload identifier instead of a multipart file, for example
[script invocation](scripts-endpoint.md#script-invocation-from-staged-uploads). Consumption is
only possible for a complete upload in state `UPLOADING`, owned by the requesting user; otherwise
the consuming endpoint returns `409` (or `404` for another user's upload). Several uploads can be
consumed by one request.

The consuming endpoint records the link of the created resource on every upload it consumed **as
soon as that resource exists**, before anything that can still fail. Repeating the same request
afterwards, including after a lost HTTP response, returns that same resource and never creates a
second one; changed parameters in a repeated request do not modify it. If the resource was deleted
since, the response is `404`.

State meanings:

| State | Meaning |
| --- | --- |
| `UPLOADING` | Chunks may be uploaded; no consumption has begun. |
| `CONSUMING` | A consuming request has begun. |
| `CONSUMED` | Consumption succeeded and the staged data was removed; `result` links to what was created. |
| `FAILED` | Consumption failed. A known `result` still identifies the resource created before the failure. |

There is a narrow crash window between creating the resource and publishing its receipt. A
`CONSUMING` or `FAILED` upload without a `result` is deliberately not retried automatically:
consuming it again returns `409`, and an administrator must inspect existing resources before
starting over. This is not a distributed transaction or an exactly-once guarantee across arbitrary
storage failure.

## Cancel staging

**DELETE /api/core/uploads/<:uuid>**

Returns `204 No Content` after discarding an `UPLOADING` upload. Stop any in-flight transfer first.
Cancellation after consumption has begun returns `409`; it never deletes what was created.

## Browser presentation

The client owns upload percentage and approximate upload speed. The transfer rate is computed from
browser upload events over a short rolling window, includes retransmitted bytes and waiting time,
and declines to zero during a stall. Hide the rate after transfer or on interruption.

Reserve 100% upload completion for acknowledgement of the final chunk, then show a distinct
"starting" phase while the consuming request runs. Upload completion is not completion of whatever
the consuming endpoint starts; that resource owns its own status.

The reference client retains the selected File and receipt during navigation within the same
application instance. Browser reload, tab closure and restarting the browser lose that client
state; persistence or automatic cross-tab/cross-user resumption is not provided.

## Deployment and cleanup

`staged-upload.directory` defaults to `${dspace.dir}/var/staged-uploads`. It must be dedicated to
staging, writable only by the service account, and shared by backend nodes serving the same
repository. The filesystem must support Java file locks and atomic rename within a directory.
Deployment with independent per-node disks needs routing affinity instead; transparent failover is
not supported without shared staging. Do not expose this directory through a web server.

`staged-upload.max-active-per-user` defaults to eight staging slots per user. Failed consumptions
retaining data continue to occupy a slot. `staged-upload.retention-hours` defaults to 24 hours;
hourly cleanup removes expired staging and receipts while skipping locked uploads. Accepted chunks
refresh retention; reading progress alone does not. Receipts are not permanent idempotency keys.

Allow up to twice the file size while chunks are assembled, plus whatever storage the consuming
endpoint needs. Early size validation is not a reservation of physical disk space: disk failure,
quota exhaustion, network failure or invalid content can still prevent the consuming endpoint from
succeeding. Reverse proxies must allow the negotiated chunk size and suitable request timeouts.
