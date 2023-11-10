# Submission COAR Notify Endpoints
[Back to the list of all defined endpoints](endpoints.md)

Provide access to the existing configurations for the submission COAR Notify panel.
The SubmissionCOARNotifyConfig list which request patterns can be selected during the submission.

In the first implementation, a single configuration named *coarnotify* is expected as these configurations were set at the site level.
Introducing an endpoint to manage a collection of configurations we open the door to future extension where different setups can be used for different kind of submissions (theses, technical reports, journal articles, etc.)

## Main Endpoint
**/api/config/submissioncoarnotifyconfigs**

Returns the list of configured COAR notify patterns to be offered during the submission.

```json
{
  "_embedded" : {
    "submissioncoarnotifyconfigs" : [ {
      "id" : "coarnotify",
      "patterns" : [ "request-review", "request-endorsement", "request-ingest" ],
      "type" : "submissioncoarnotifyconfig",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/submissioncoarnotifyconfigs/coarnotify"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/submissioncoarnotifyconfigs"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated. The endpoint is restricted to authenticated users


## Single COAR notify
**/api/config/submissioncoarnotifyconfigs/<:coarnotify-name>**

*:coarnotify-name* is initially hard-coded to *coarnotify*

Provide detailed information about a specific COAR notify.
```json
{
  "id" : "coarnotify",
  "patterns" : [ "request-review", "request-endorsement", "request-ingest" ],
  "type" : "submissioncoarnotifyconfig",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/submissioncoarnotifyconfigs/coarnotify"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated. The endpoint is restricted to authenticated users
* 404 Not found - if the requested submission COAR Notify Config doesn't exist