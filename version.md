# Versioning endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint provide access to a single version, matching a single item.

Whether a version is accessible depends on `versioning.item.history.view.admin` configuration.  
If this is set to true, the version can only be retrieved if the user is a community, collection or repository administrator, otherwise the versions are public.
Please note that the security of the single version is in no way related to the linked item. 
It is eventually possible to retrieve a version related to a not accessible item, as the version doesn't expose any sensitive information this pose no security risks.

## Main Endpoint
**/api/versioning/versions**   

_Unsupported._ Versions can only be navigated starting from a specific item, version history or accessed directly by id

Status codes:
* 405 Method Not Allowed

## Create version

**POST /api/versioning/versions[?summary=<summary-text>]**

* summary: it is an optional textual parameter that if provided will be saved in the summary property of the new version

Item administrators or, according to the `versioning.submitterCanCreateNewVersion` configuration, the original submitter can create a new version of an item. The content-type is uri-list.

The URI-list should contain the uri of the item that should be used to create the new version. It can be an item without an existing version history or an item part of an existing history. In the latest case it is possible to use an item representing an older version than the current one to facilitate the restore.

The new created version will be returned in the response, see the [get single version](#get-single-version) endpoint for an example of response.

An example curl call:

```
 curl -i -X POST https://demo7.dspace.org/server/api/versioning/versions \
 -H "Content-Type:text/uri-list" \
 --data "https://demo7.dspace.org/server/api/core/items/a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9"
```

Status codes:
* 201 Created - if the new version has been created
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions or if you are trying to create a new version for an entity item and the `versioning.block.entity` configuration property is `true` or unset
* 400 Bad Request - if the uri doesn't resolve to an item
* 422 Unprocessable Entity - if there are already an inprogress submission representing a new version in the same version history of the item 

## Get single version

**GET /api/versioning/versions/<:versionId>**

Provide the version information associated to a specific id. The returned attributes depend on the user permissions and system configuration.
The following example shows all the available attributes as exposed to an administrator

```json
{
  "id": "345",
  "version": "101",
  "type": "version",
  "created": "2015-11-03T09:44:46.617",
  "summary": "Fixing some typos in the abstract",
  "submitterName": "LastName, FirstName",
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

The `submitterName` attribute is only exposed if you are logged in as an administrator of the linked item or if the `versioning.item.history.include.submitter` configuration property is set to true.

Status codes:
* 200 OK - if the version exists and is accessible by the current user
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if the version doesn't exist or was deleted

### Get single version for item
Please follow the version link in [the item endpoint](items.md#get-single-version-for-item)

## Remove version

To delete a version you need to [delete the item](items.md#deleting-an-item) linked to it.

## Update version (patch operations)

Only the summary property can be updated by item administrators, all the other properties are **READ-ONLY**. Attempt to update any other information will result in a 422 error.

To update the metadata of the corresponding item the appropriate method in the items, workspaceitems or workflowitems endpoints need to be used according to the item status.

**PATCH**

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
