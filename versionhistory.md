# Version history endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint represents the version history that group all related versions.

Whether version history information is accessible depends on versioning.item.history.view.admin configuration
If this is set to true, the version history can only be retrieved if the user is a community, collection or repository administrator, otherwise the version histories are public.
Please note that the security of the version history is in no way related to any of the items that belong to it. 
It is eventually possible to retrieve an history containing only not accessible items. As the version history neither the single version expose any sensitive information this pose no security risks.

## Main Endpoint
**/api/versioning/versionhistories**   

_Unsupported._ Version histories can only be navigated starting from a specific version item or accessed directly by id

Status codes:
* 405 Method Not Allowed

## Get version history

**GET /api/versioning/versionhistories/<:versionHistoryId>**

Provide information about for a version history id.

```json
{
  "id": "1",
  "draftVersion": false,
  "type": "versionhistory",  
  "_links": {
    "self": {
      "href": "https://demo7.dspace.org/server/api/versioning/versionhistories/1"
    },
    "versions": {
      "href": "https://demo7.dspace.org/server/api/versioning/versionhistories/1/versions"
    },
    "draftVersion": {
      "href": "https://demo7.dspace.org/server/api/versioning/versionhistories/1/draftVersion"
    }
  }
}
```
Attributes:
- draftVersion: true, if the most recent version is associated to an item still in progress (workspace or workflow), false otherwise. This attribute is only visible to users allowed to create a new version in the history, see [Create version endpoint](version.md#Create version) 

Status codes:
* 200 OK - if the version history exists and is accessible by the current user
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if you have the permission to review the version history and it doesn't exist

## Linked entities

### Versions

**GET /api/versioning/versionhistories/<:versionHistoryId>/versions**

Retrieve a pageable list of versions for the provided version history identifier.  
The versions are ordered by version number descending and attributes are secured as described in the [get single version endpoint](version.md#get-single-version).
Only versions related to archived or withdrawn items are return, the most recent version will be excluded from this list if it is not yet archived.

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
       "href": "https://demo7.dspace.org/server/api/versioning/versions/search/findByHistory?historyId=1"
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

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public

### draftVersion

**GET /api/versioning/versionhistories/<:versionHistoryId>/draftVersion**

If the latest version is not yet archived, provide access to the workspaceitem or workflowitem associated with it.
The link will respect the security defined in the workspaceitem or workflowitem endpoint so it could be not visible to user otherwise allowed to create a new version in the history.
For this scenario the `draftVersion` flag allows the UI to turn off the "create new version" button. 
Due to the restriction applied to this flag anonymous users will be unable to discover that a new version is currently under review.
