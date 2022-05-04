# Suggestion Sources Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/suggestionsources**   

It returns a paginated list of suggestion sources. All the sources are returned regardless to the existence or less of suggestions. This endpoint is reserved to administrators

### single entry
**GET api/integration/suggestionsources/<:source-key>**

It returns the data from a specific source. This endpoint is reserved to administrators

sample for a source /api/integration/suggestionsources/reciter
```json
{
    "id": "reciter",
    "total": 2,
    "type": "suggestionsource",
    "_links": {
      "suggestiontargets": {
        "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/search/findBySource?source=reciter"
      },
      "self": {
        "href": "https://dspace7.4science.cloud/server/api/integration/suggestionsources/reciter"
      }
    }
}
```

Attributes
* the *id* attribute is the key that identify the source
* the *total* attribute is the number of target with suggestions. It can be 0 if there are no target with suggestions

Exposed links:
* suggestiontargets: link to the suggestion targets entries

Status codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the source doesn't exist or is not active in the system (when 403 doesn't apply) 
