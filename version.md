# Versioning endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint represents a single version, matching a single item.

Whether a version is accessible depends on `versioning.item.history.view.admin` configuration.  
If this is set to true, the version can only be retrieved if the user is an admin of the related item

## Create version

**POST /api/versioning/versions[?summary=<summary-text>]**

* summary: it is an optional textual parameter that if provided will be saved in the summary property of the new version

Item administrators or, according to the `versioning.submitterCanCreateNewVersion` configuration, the original submitter can create a new version of an item. The content-type is uri-list.

The URI-list should contain the uri of the item that should be used to create the new version. It can be an item without an existing version history or an item part of an existing history. In the latest case it is possible to use an item representing an older version than the current one to facilitate the restore. 

An example curl call:

```
 curl -i -X POST https://demo7.dspace.org/server/api/versioning/versions \
 -H "Content-Type:text/uri-list" \
 --data "https://demo7.dspace.org/server/api/core/items/a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9"
```

Status codes:
* 200 OK - if the new version has been created
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 400 Bad Request - if the uri doesn't resolve to an item
* 422 Unprocessable Entity - if there are already an inprogress submission representing a new version in the same version history of the item 

## Get single version

**GET /api/versioning/versions/<:versionId>**

Provide version information for the version id.

```json
{
  "id": "345",
  "version": "101",
  "type": "version",
  "created": "2015-11-03T09:44:46.617",
  "summary": "Fixing some typos in the abstract",
  "_links": {
    "versionhistory": {
      "href": "https://demo7.dspace.org/server/api/versioning/versionhistories/1"
    },
    "self": {
      "href": "https://demo7.dspace.org/server/api/versioning/versions/345"
    },
    "item": {
      "href": "https://demo7.dspace.org/server/api/versioning/versions/345/item"
    }
  }
}         
```

Status codes:
* 200 OK - if the version exists and is accessible by the current user
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if the version doesn't exist or was deleted

### Get single version for item

**GET /api/core/items/{:item-uuid}/version**

See [the item endpoint](items.md#get-single-version-for-item)

### Search methods
#### findByItem
**/api/versioning/versions/search/findByItem**

The supported parameters are:
* `itemUuid`: mandatory, the item uuid

It returns the single version, if any, related to the specified item.

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 204 No Content - if the specified item is not yet versioned
* 400 Bad Request - if the item id param is missing or invalid (not an uuid)

#### findByHistory
**/api/versioning/versions/search/findByHistory**

The supported parameters are:
* `historyId`: mandatory, the version history id
* `page`, `size` [see pagination](README.md#Pagination)

It returns a pageable list of not yet deleted versions for the provided version history identifier.  
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
* 204 No Content - if there are no versions related to the specified history
* 400 Bad Request - if the history id param is missing or invalid (not an Integer)

## Remove version

**DELETE /api/versioning/versions/<:versionId>**

Delete a version item.

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist (or was already deleted)

## Update version (patch operations)

Only the summary property can be updated by item administrators, all the other properties are **READ-ONLY**. Attempt to update any other information will result in a 422 error.

To update the metadata of the corresponding item the appropriate method in the items, workspaceitems or workflowitems endpoints need to be used according to the item status.

**PATCH **

To update the summary property a patch request over the `/summary` path is needed.
According to the [general patch rules](patch.md)
To nullify a summary use the `"op": "remove"` patch operation
To replace an existing summary use the `"op": "replace"` or `"op": "add"` patch operation
To set a summary value when the summary is null use the `"op": "add"` patch operation

For example, `curl -X PATCH http://${dspace.url}/api/versioning/versions/<:id-version> -H "Content-Type: application/json" -d '[{ "op": "add", "path": "/summary", "value": "This is the note related to the version"]'`.  The operation also requires an Authorization header.

Return codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the version doesn't exist (or was deleted)
* other error codes according to the [general patch operation rules](patch.md)

## Linked entities

### Version History

Retrieves the related version history, for details see [Version history](versionhistory.md)

### Item

The [Item](items.md) matching this specific version
