# Items Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/items**   

Provide access to the items (DBMS based). This endpoint is reserved to the administrators the common practice is to navigate to the single items via a discovery [search](search-endpoint.md) or [browse](browses.md).

Currently, this endpoint only returns "archived" items which are NOT withdrawn. However, withdrawn items and Workspace/Workflow items are findable via [search endpoints](search-endpoint.md). Template items are findable via the corresponding collection. In the future, this endpoint is likely to be updated to include all items, regardless of status, see https://github.com/DSpace/DSpace/issues/3325

Example: <https://demo.dspace.org/server/#/server/api/core/items>

## Single Item
**/api/core/items/<:uuid>**

***
:warning: In the below example response, the existence of the `place` field for specific metadata values is still under analysis. We are determining whether it can be removed entirely in favor of using the array index (as the `place` field represents the index of each value in an ordered array). For more details see Github issue https://github.com/DSpace/DSpace/issues/7582
***

Provide detailed information about a specific item. The JSON response document is as follow
```json
{
  "uuid": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
  "name": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
  "handle": "10673/20",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Stvilia, Besiki",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      },
      {
        "value": "Lee, Dong Joon",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 1
      }
    ],
    "dc.identifier.url": [
      {
        "value": "http://europepmc.org/abstract/MED/28301533",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "dc.title": [
      {
        "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2017-06-24T00:40:54.970+0000",
  "type": "item"
}
```

Item properties:
* handle: the handle identifier assigned to the item, if any
* metadata: a map of all the item's metadata where the metadata field is used to build the key in the form of <schema>.<element>[.<qualifier] and the values are an ordered list of metadata values. Administrative metadata, such as dc.description.provenance can be filtered out and available only to administrators according to the system configuration, see metadata.hide.* properties in the dspace.cfg file. For withdrawn items the actual metadata are only shown to administrators, all the other users will get an empty property
* inArchive: it will be true only for items that have been successful deposited in the repository and have passed all, if any, the defined workflow steps. For item related to workspaceitem, workflowitem or template items this attribute will stay at false
* discoverable: true if the item is expected to be findable trough a search or browse by authorized user
* withdrawn: true it the item has been retired from the repository

Exposed links:
* bundles: list of bundles within the item
* owningCollection: the collection where the item belong to
* mappedCollections: the collections where the item is mapped to
* templateItemOf: the collection that have the item as template
* relationships: the relationships to other items
* thumbnail: the main thumbnail of the item

Status codes:
* 200 OK - if the item is found and it is visible to the current user or the anonymous user. Withdrawn items are returned
* 401 Unauthorized - if you are not authenticated and the item is not visible to anonymous users
* 403 Forbidden - if you are not logged in with sufficient permissions. Please note that withdrawn items are visible to everyone without any metadata details
* 404 Not found - if the item doesn't exist

### Withdrawn item
A withdrawn item is a normal item that has been retired from the repository using a patch operation as described below. Once withdrawn the response will show an empty metadata section to everyone except than administrators.

 ```
 {
    "id":"e45a63a2-3db2-47fa-824e-3e4fdfae94f1",
    "uuid":"e45a63a2-3db2-47fa-824e-3e4fdfae94f1",
    "name":"Public item 1",
    "handle":"123456789/3",
    "metadata":{
    },
    "inArchive":false,
    "discoverable":true,
    "withdrawn":true,
    "lastModified":"2021-06-21T12:53:57.871+00:00",
    "entityType":null,
    "type":"item",
    "_links":{
        ...
    }
}
```


## Creating an archived item
**POST /api/core/items?owningCollection=<:uuid>**

Administrators can directly create an archived item (bypassing the workflow). The content-type is JSON. An example JSON can be seen below:

```json
{
  "name": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Stvilia, Besiki",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "type": "item"
}
```

### Creating an archived item from an external source
**POST /api/core/items?owningCollection=<:uuid>**

Administrators can directly create an archived item (bypassing the workflow) from an external source. The content-type is uri-list.

The URI-list should contain the [external entry value](external-authority-sources.md) whose metadata should be imported

An example curl call:
```
 curl -i -X POST https://demo.dspace.org/server/api/core/items?owningCollection=1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb \
 -H "Content-Type:text/uri-list" \
 --data "https://demo.dspace.org/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
```

Only one external entry value should be present. If multiple external entry values are present, a 400 bad request will be thrown

## Updating item metadata

**PUT /api/core/items/<:uuid>**

Provide updated metadata information for an item, when the update is completed the updated object will be returned. The JSON to update can be found below.

```json
{
  "uuid": "a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9",
  "handle": "123456789/60636",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Velasco, Mercedes",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Test new title",
        "language": "pt_BR",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "type": "item"
}
```
 
## Patch operations

Item metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

Additional properties can be modified via Patch as described below.

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To withdraw an item, `curl --data '[ { "op": "replace", "path": "/withdrawn", "value": true}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The withdraw operation also requires an Authorization header.

For example, starting with the following item data:
```json
 "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the withdraw operation will result in:
```json
  "inArchive": false,
  "discoverable": true,
  "withdrawn": true,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```

To reinstate an item, `curl --data '[ { "op": "replace", "path": "/withdrawn", "value": false}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The reinstate operation also requires an Authorization header.

For example, starting with the following item data:
```json
 "inArchive": false,
  "discoverable": true,
  "withdrawn": true,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the reinstate operation will result in:
```json
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```

To make an item private (or discoverable), `curl --data '[ { "op": "replace", "path": "/discoverable", "value": false}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The discoverable operation also requires an Authorization header.


For example, starting with the following item data:
```json
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the discoverable operation will result in:
```json
  "inArchive": true,
  "discoverable": false,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
## Linked entities

### Access Status

**GET /api/core/items/<:uuid>/accessStatus**

This endpoint expose the mechanism for retrieving and calculating the access status of a DSpace item.
It can be checked by calling this endpoint with the the corresponding item UUID.

```
curl -v "http://{dspace-server.url}/api/core/items/2245f2c5-1bed-414b-a313-3fd2d2ec89d6/accessStatus"
```

This will return the access status, E.G.:

_200 - Response if the UUID parameter is valid_
```json
{
  "status": "metadata.only",
  "type": "accessStatus",
  "_links" : {
    "self" : {
      "href" : "http://{dspace-server.url}/api/core/items/2245f2c5-1bed-414b-a313-3fd2d2ec89d6/accessStatus"
    }
  }
}
```

Fields
- Status: String value if the UUID is valid
- Type: Type of the endpoint, "accessStatus" in this case

Exposed links:
- self: The valid URL to the item's access status

Default access status values
- metadata.only = Item doesn't contain a primary file
- open.access = Item's primary file is downloadable to anonymous users
- embargo = Item's primary file is under an embargo
- restricted = Item's primary file is not downloadable to anonymous users
- unknown = Item's status is indeterminable or unknown

_Note: the calculation of those default values is based on the policies of the item's primary file._
_The term primary file also refers to the first file in the original bundle if no primary file is defined._

Return code
- 200 Ok if the parameter is a valid item UUID
- 400 Bad Request if the parameter is invalid
- 404 Not Found if the item cannot be retrieved 

### Bundles

**GET /api/core/items/<:uuid>/bundles**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bundles>

It returns the bundles within this item. See the [bundle endpoint](bundles.md) for more info

```json
{
  "bundles" : [
  {
    "uuid": "d3599177-0408-403b-9f8d-d300edd79edb",
    "name": "ORIGINAL",
    "handle": null,
    "metadata": {},
    "type": "bundle",
    "_links" : {
      "primarybitstream" : {
        "href" : "https://demo.dspace.org/server/api/core/bitstreams/ac49f361-4ffd-47a4-8eb2-e6c73c3f3e76"
      },
      "bitstreams" : {
        "href" : "https://demo.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams"
      },
      "self" : {
        "href" : "https://demo.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb"
      }
    }
  },
  {
    "uuid": "d3599177-0408-403b-9f8d-d300edd79edb",
    "name": "THUMBNAIL",
    "handle": null,
    "metadata": {},
    "type": "bundle",
    "_links" : {
      "primarybitstream" : {
        "href" : "https://demo.dspace.org/server/api/core/bitstreams/ac49f361-4ffd-47a4-8eb2-e6c73c3f3e76"
      },
      "bitstreams" : {
        "href" : "https://demo.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams"
      },
      "self" : {
        "href" : "https://demo.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb"
      }
    }
  }
  ]
}
```

This endpoint is relevant to:
* Retrieve only the bitstreams from a given bundle from an item (e.g. only the thumbnails)
* Retrieve or update the order of the bitstreams in a bundle

**POST /api/core/items/<:uuid>/bundles**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bundles>

Creating a new bundle in an item would use JSON similar to the example below:

```json
{
  "name": "ORIGINAL",
  "metadata": {}
}
```

It returns the created bundle.

If a bundle with the given doesn't exist yet in the item, it will be created

Status codes:
* 201 Created - if the operation succeed
* 400 Bad Request - if the bundle name already exists in the item
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist
* 412 Precondition Failed - if there is a discrepancy between the declared size or checksum and the computed one

### Owning Collection
**/api/core/items/<:uuid>/owningCollection**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection>

It returns the collection where the item belong to

**PUT /api/core/items/<:uuid>/owningCollection**

The actual collection is part of the body using the uri-list. Note that if the parameter inheritPolicies=true is passed in the request, the item will inherit the policies of the target owning collection.

Example:

```curl -i -X PUT "https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection" -H "Content-Type:text/uri-list" -d "https://demo.dspace.org/server/api/core/collections/8e0928a0-047a-4369-8883-12669f32dd64"```

It updates the owning collection (moves the item).

Status codes:
* 204 No content - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist

### Mapped Collections
**GET /api/core/items/<:uuid>/mappedCollections**

It returns all the mapped collections the item is included in.

On the item page, it should be referenced similar to:
```json
    "mappedCollections": {
      "href": "https://demo.dspace.org/server/api/core/items/95e5d7d9-ef4e-4e35-86cc-07bfe2f0e355/mappedCollections"
    }
```

**GET /api/core/items/<:uuid>/mappedCollections/<:collection_uuid>**

_Unsupported._ If you want detailed information about a single mapped collection, use the `/api/core/collections/<collection:uuid>` endpoint.

**POST /api/core/items/<:uuid>/mappedCollections**

A POST request will result in creating a new mapping between the item and collection.
If the collection exists and is neither the owning nor mapped collection for the item, the relation should be created.

```
 curl -i -X POST https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections 
 -H "Content-Type:text/uri-list" --data "https://demo.dspace.org/server/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
```

The collection(s) MUST be included in the body using the `text/uri-list` content type
 
Return codes:
 * 204: if the update succeeded (including the case of no-op if the mapping was already as requested)
 * 401  Unauthorized - if you are not authenticated
 * 403  Forbidden - if you are not logged in with sufficient permissions 
 * 405: if the item is a template item
 * 422: if the specified collection is not found or is the owningCollection of the item

**PUT /api/core/items/<:uuid>/mappedCollections**

_Unsupported._ You may replace or update mapped collections using DELETE requests and/or POST requests.

**DELETE /api/core/items/<:uuid>/mappedCollections**

_Unsupported._ At this time, we do not support removing all mapped Collections in a single request. Please use `DELETE /api/core/items/<:uuid>/mappedCollections/<:collection_uuid>` to remove mapped Collections one by one.

**DELETE /api/core/items/<:uuid>/mappedCollections/<:collection_uuid>**

A DELETE request will result in removing an existing mapping between the item and collection.
If the collection exists and is a mapped collection for the item, the relation should be deleted.

```
 curl -i -X DELETE https://demo.dspace.org/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
```

The above request would remove the mapping between Collection with UUID `1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb` and Item with UUID `1911e8a4-6939-490c-b58b-a5d70f8d91fb`.

Return codes:
 * 204: if the delete succeeded (including the case of no-op if the collection was not mapped) 
 * 401  Unauthorized - if you are not authenticated
 * 403  Forbidden - if you are not logged in with sufficient permissions 
 * 405: if the item is a template item
 * 422: if the specified collection is not found or is the owningCollection of the item


### Template Item
**/api/core/items/<:uuid>/templateItemOf**

Example: to be provided

It returns the collection that have the item as template

### Relationships per item
**/api/core/items/<:uuid>/relationships**

A sample can be found at https://demo.dspace.org/#https://demo.dspace.org/server/api/core/items/5a3f7c7a-d3df-419c-b8a2-f00ede62c60a/relationships

It embeds all relationships where either the left or the right item matches the given uuid

### Main Thumbnail
**/api/core/items/<:uuid>/thumbnail**

It returns the bitstream which represents the main thumbnail of the item

Status codes:
* 200 OK - returning the thumbnail
* 204 No Content - if there is no thumbnail for the specified item
* 401 Unauthorized - if you are not authenticated and don't have permissions on the item or the thumbnail's metadata
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist

### Get single version for item
**GET /api/core/items/{:item-uuid}/version**

Provide version information based on a given Item UUID. An Item UUID will only match one version. READ permissions over the item in addition to the version permissions are checked.
The JSON response is the same as the [Version endpoint](versions.md#get-single-version).

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 204 No Content - if the specified item is not yet versioned
* 400 Bad Request - if the item id param is missing or invalid (not an uuid)

### Get item identifiers
**GET /api/core/items/{:item-uuid}/identifiers**

Returns information about the identifiers associated with this item, for example Handle and DOI URIs. If relevant, the status of the identifier is also included.

The JSON response is formatted like the example below (the same data model as the [identifiers submission step](submissionsections.md)).
```json
{
  "identifiers" : [ {
    "value" : "https://doi.org/10.33515/dspace-61",
    "identifierType" : "doi",
    "identifierStatus" : "TO_BE_REGISTERED",
    "type" : "identifier"
  }, {
    "value" : "123456789/418",
    "identifierType" : "handle",
    "identifierStatus" : null,
    "type" : "identifier"
  } ],
  "type" : "identifiers",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/server/api/core/items/6bea2772-0e71-4636-8c1c-5c132821fa2c/identifiers"
    }
  }
}
```
Return codes:
* 200 OK - if the operation succeeds
* 400 Bad Request - if the item id param is missing or invalid (not an uuid)
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if the item doesn't exist

### Get Item Submitter
**/api/core/items/<:uuid>/submitter**

It returns the submitter of the item

Status codes:
* 200 OK - returning the submitter
* 204 No Content - if you are not authenticated or you have no read access on that submitter.
* 404 Not found - if the item doesn't exist

## Deleting an item

**DELETE /api/core/items/<:uuid>**

Delete an item.

An optional parameter for copying virtual metadata to actual metadata in the related items can be included (only authorized by admins): `copyVirtualMetadata`. This can contain values:
* all (all relationships are verified, and the virtual metadata in all related items is migrated to actual metadata)
* relationship type ID: only relationship types with the given ID(s) are migrated. The `copyVirtualMetadata` can be included multiple times to support multiple IDs
* configured: the behavior will be retrieved from a configuration parameter
* _not specified_: no virtual metadata is expanded to actual metadata

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist (or was already deleted)
