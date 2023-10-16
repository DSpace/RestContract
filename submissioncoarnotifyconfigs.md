# Submission COAR Notify Endpoints
[Back to the list of all defined endpoints](endpoints.md)

The proposed structure is derived from the configuration options already available in DSpace 6 and below.
In the first implementation, a single configuration named *default* is expected as these configurations were set at the site level.
Introducing an endpoint to manage a collection of configurations we open the door to future extension where different setups can be used for different kind of submissions (theses, technical reports, journal articles, etc.)

## Main Endpoint
**/api/config/submissioncoarnotifyconfigs**

Returns the list of configured COAR notify.

```json
{
  "_embedded" : {
    "submissioncoarnotifyconfigs" : [ {
      "id" : "default",
      "patterns" : [ "review", "endorsement", "ingest" ],
      "type" : "submissioncoarnotifyconfig",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/submissioncoarnotifyconfigs/default"
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
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users

## Single COAR notify
**/api/config/submissioncoarnotifyconfigs/<:coarnotify-name>**

*:coarnotify-name* is initially hard-coded to *default*

Provide detailed information about a specific COAR notify.
```json
{
  "id" : "default",
  "patterns" : [ "review", "endorsement", "ingest" ],
  "type" : "submissioncoarnotifyconfig",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/submissioncoarnotifyconfigs/default"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users
* 404 Not found - if the submission COAR Notify Config doesn't exist