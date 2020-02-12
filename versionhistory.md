# Version history endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint represents all related versions.

Whether version history information is accessible depends on versioning.item.history.view.admin configuration
If this is set to true, the version history can only be retrieved if the user is an admin of the last version's item

## Get version history

**GET /api/versioning/versionhistories/<:versionHistoryId>**

Provide version information for the version history id.

```json
{
  "id": "1",
  "type": "versionhistory",
  "_links": {
    "self": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/versioning/versionhistories/1"
    },
    "versions": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/versioning/versionhistories/1/versions"
    }
  }
}
```

Status codes:
* 200 OK - if the version history exists and is accessible by the current user
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if the version history doesn't exist

## Linked entities

### Versions

**GET /api/versioning/versionhistories/<:versionHistoryId>/versions**

Retrieve a pageable list of versions for the provided version history identifier.  
The versions are ordered by version number descending.

```json
{
  "_embedded": {
    "versions": [
      {
          "id": "102",
          "version": "2",
          "type": "version",
          "created": "2019-10-31T09:44:46.617",
          "summary": "Author order"
      },
      {
        "id": "101",
        "version": "1",
        "type": "version",
        "created": "2015-11-03T09:44:46.617",
        "summary": "Fixing some typos in the abstract"
      }
    ],
    "_links": {
     "self": {
       "href": "https://dspace7.4science.it/dspace-spring-rest/api/versioning/versionhistories/1/versions"
     }
    },
    "page": {
     "size": 20,
     "totalElements": 2,
     "totalPages": 1,
     "number": 0
    }
  }
}
```
