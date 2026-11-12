# Metrics Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/cris/metrics**

As we don't have a use case to iterate over all the existing metrics, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

The metrics of a single item are exposed through the paginated link endpoint documented in [items.md](items.md#metrics).

## Single Metric
**/api/cris/metrics/<:id>**

Provide detailed information about a specific metric. The `id` is a string, not a number, and can have two different forms:

* `storedmetric-<n>` for a metric stored in the database, where `<n>` is the numeric identifier of the stored metric (e.g. `storedmetric-1`).
* `<item-uuid>:<metric-type>` for a metric provided dynamically by an *embeddable* metric provider, i.e. a metric that is computed on the fly from the item and is not persisted. Examples are `1911e8a4-6939-490c-b58b-a5d70f8d91fb:embedded-view` or `1911e8a4-6939-490c-b58b-a5d70f8d91fb:google-scholar`.

Clients must use the `id` exactly as returned by the API, including the `storedmetric-` prefix or the `<uuid>:` part. A bare number or a bare UUID is not a valid identifier.

An example JSON response document to `/api/cris/metrics/storedmetric-1`:
```json
{
    "id": "storedmetric-1",
    "metricType": "scopusCitation",
    "metricCount": 4.0,
    "acquisitionDate": "2019-10-31T00:00:00.000+00:00",
    "startDate": null,
    "endDate": null,
    "last": true,
    "remark": "{\"identifier\":\"2-s2.0-67349162500\",\"pmid\":\"19406218\",\"doi\":\"10.1016/j.gene.2009.04.019\"}",
    "deltaPeriod1": 3.0,
    "deltaPeriod2": 12.0,
    "rank": 50.0,
    "type": "metric",
    "uniqueType": "cris.metric",
    "_links": {
        "self": {
            "href": "https://{dspace.server.url}/api/cris/metrics/storedmetric-1"
        }
    }
}
```

The attributes are:

* `id` - the metric identifier, as described above
* `metricType` - the type of the metric (e.g. `scopusCitation`, `wosCitation`, `embedded-view`, `embedded-download`, `altmetric`)
* `metricCount` - the value of the metric, as a decimal number
* `acquisitionDate` - when the metric value was acquired, in ISO-8601 format
* `startDate` - the start of the period the metric refers to, in ISO-8601 format, or `null`
* `endDate` - the end of the period the metric refers to, in ISO-8601 format, or `null`
* `last` - `true` if this is the most recent value recorded for the metric type, `false` otherwise
* `remark` - a free-text note. It is a **string**, even when its content is itself a serialized JSON object (as in the example above): clients must parse it as a string first and, if needed, parse the resulting text as JSON
* `deltaPeriod1` - the change of the metric over the first comparison period, or `null`
* `deltaPeriod2` - the change of the metric over the second comparison period, or `null`
* `rank` - the rank of the metric, or `null`
* `type` - always `metric`
* `uniqueType` - always `cris.metric`
* `_links.self` - the link to this metric

None of the metric attributes is annotated with `@JsonInclude(NON_NULL)`, so every attribute is always present in the response, even when its value is `null`. The following example, for a dynamically provided metric, shows the attributes that are typically `null` for this kind of metric:
```json
{
    "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb:embedded-view",
    "metricType": "embedded-view",
    "metricCount": null,
    "acquisitionDate": null,
    "startDate": null,
    "endDate": null,
    "last": false,
    "remark": "http://localhost:4000/statistics/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb",
    "deltaPeriod1": null,
    "deltaPeriod2": null,
    "rank": null,
    "type": "metric",
    "uniqueType": "cris.metric",
    "_links": {
        "self": {
            "href": "https://{dspace.server.url}/api/cris/metrics/1911e8a4-6939-490c-b58b-a5d70f8d91fb:embedded-view"
        }
    }
}
```

Status codes:

* 200 OK - if the metric exists and the user has sufficient permissions
* 401 Unauthorized - if the metric is not accessible to the Anonymous user and the request is not authenticated
* 403 Forbidden - if the user is authenticated but does not have sufficient permissions. Access requires the READ permission on the item related to the metric and the metric being exposed in one of the metrics boxes configured for the item's entity type (when no metrics box is configured for that entity type, this second condition is not enforced); administrators can always access it
* 404 Not Found - if the metric doesn't exist (or was already deleted)
