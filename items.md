# Items Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/items**   

Provide access to the items (DBMS based). It returns the list of existent items.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/items>

## Single Item
**/api/core/items/<:uuid>**

***
:warning: In the below example response, the existence of the `place` field for specific metadata values is still under analysis. We are determining whether it can be removed entirely in favor of using the array index (as the `place` field represents the index of each value in an ordered array). For more details see https://jira.duraspace.org/browse/DS-4242
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

Exposed links:
* bitstreams: list of bitstreams within the item
* owningCollection: the collection where the item belong to
* mappedCollections: the collections where the item is mapped to
* templateItemOf: the collection that have the item as template
* relationships: the relationships to other items
 
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
 curl -i -X POST https://dspace7.4science.it/dspace-spring-rest/api/core/items?owningCollection=1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb \
 -H "Content-Type:text/uri-list" \
 --data "https://dspace7.4science.it/dspace-spring-rest/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
```

Only one external entry value should be present. If multiple external entry values are present, a 400 bad request will be thrown

## Updating item metadata

**PUT /api/core/items/<:uuid>**

Provide updated metadata information for an item, when the update is completed the updated object will be returned. The JSON to update can be found below.

```json
{
  "id": "a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9",
  "uuid": "a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9",
  "name": "Test new title",
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
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
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

### Bundles

**GET /api/core/items/<:uuid>/bundles**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bundles>

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
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bitstreams/ac49f361-4ffd-47a4-8eb2-e6c73c3f3e76"
      },
      "bitstreams" : {
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams"
      },
      "self" : {
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb"
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
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bitstreams/ac49f361-4ffd-47a4-8eb2-e6c73c3f3e76"
      },
      "bitstreams" : {
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams"
      },
      "self" : {
        "href" : "https://dspace7-entities.atmire.com/rest/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb"
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

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bundles>

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

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection>

It returns the collection where the item belong to

**PUT /api/core/items/<:uuid>/owningCollection**

The actual collection is part of the body using the uri-list
Example:

```curl -i -X PUT "https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection" -H "Content-Type:text/uri-list" -d "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/8e0928a0-047a-4369-8883-12669f32dd64"```

It updates the owning collection (moves the item)

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
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/core/items/95e5d7d9-ef4e-4e35-86cc-07bfe2f0e355/mappedCollections"
    }
```

**GET /api/core/items/<:uuid>/mappedCollections/<:collection_uuid>**

_Unsupported._ If you want detailed information about a single mapped collection, use the `/api/core/collections/<collection:uuid>` endpoint.

**POST /api/core/items/<:uuid>/mappedCollections**

A POST request will result in creating a new mapping between the item and collection.
If the collection exists and is neither the owning nor mapped collection for the item, the relation should be created.

```
 curl -i -X POST https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections 
 -H "Content-Type:text/uri-list" --data "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
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
 curl -i -X DELETE https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
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

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/items/5a3f7c7a-d3df-419c-b8a2-f00ede62c60a/relationships

It embeds all relationships where either the left or the right item matches the given uuid

### Get single version for item
**GET /api/core/items/{:item-uuid}/version**

Provide version information based on a given Item UUID. An Item UUID will only match one version.

The JSON response and status codes are the same as the [Version endpoint](version.md#get-single-version).

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
