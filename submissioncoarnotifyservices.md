# Submission COAR Notify Endpoints
[Back to the list of all defined endpoints](endpoints.md)

The proposed structure is derived from the configuration options already available in DSpace 6 and below.
In the first implementation, a single configuration named *default* is expected as these configurations were set at the site level.
Introducing an endpoint to manage a collection of configurations we open the door to future extension where different setups can be used for different kind of submissions (theses, technical reports, journal articles, etc.)

## Main Endpoint
**/api/config/submissioncoarnotifyservices**

Returns the list of configured COAR notify.

```json
{
  "_embedded": {
    "submissioncoarnotifyservices": [
      {
        "id": "default",
        "name": "default",
        "type": "submissioncoarnotifyservices",
        "patterns": ["review", "endorsement", "ingest"]
      }
    ]
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```

## Single COAR notify
**/api/config/submissioncoarnotifyservices/<:coarnotify-name>**

*:coarnotify-name* is initially hard-coded to *default*

Provide detailed information about a specific COAR notify.
```json
{
  "id": "default",
  "name": "default",
  "type": "submissioncoarnotifyservices",
  "patterns": ["review", "endorsement", "ingest"],
  "_links" : {
    "self" : {
      "href" : "/api/config/submissioncoarnotifyservices/default"
    }
  }
}
```

### Linked entities
#### metadata
**/api/confg/submissioncoarnotifyservices/:coarnotify-name/metadata**
The [submission-form](submissionforms.md) used for the metadata of the COAR notify panel