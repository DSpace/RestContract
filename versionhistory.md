# Version history endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint represents the version history that group all related versions.

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
      "href": "https://demo7.dspace.org/server/api/versioning/versionhistories/1"
    },
    "oldestversion": {
      "href": "https://demo7.dspace.org/server/api/versioning/versions/1"
    },
    "currentversion": {
      "href": "https://demo7.dspace.org/server/api/versioning/versions/100"
    },
    "lastversion": {
      "href": "https://demo7.dspace.org/server/api/versioning/versions/101"
    }
  }
}
```

Status codes:
* 200 OK - if the version history exists and is accessible by the current user
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if you have the permission to review the version history and it doesn't exist

## Linked entities

### Oldest Version

**GET /api/versioning/versionhistories/<:versionHistoryId>/oldestversion**

Retrieve the oldest version in the history 

### Current Version

**GET /api/versioning/versionhistories/<:versionHistoryId>/currentversion**

Retrieve the most recent archived version in the history

### Last Version

**GET /api/versioning/versionhistories/<:versionHistoryId>/lastversion**

Retrieve the most recent version in the history that could live eventually in the workspace or workflow 

### Versions

To avoid a bidirectional link between the versions and the history that they belong to a search method is available in the [version endpoint](version.md) to retrieve all the versions by history id