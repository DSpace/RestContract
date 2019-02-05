# Items Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/items**   

Provide access to the items (DBMS based). It returns the list of existent items.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/items>

## Single Item
**/api/core/items/<:uuid>**

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
        "confidence": -1
      },
      {
        "value": "Lee, Dong Joon",
        "language": "en",
        "authority": null,
        "confidence": -1
      },
    ],
    "dc.identifier.url": [
      {
        "value": "http://europepmc.org/abstract/MED/28301533",
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
  "lastModified": "2017-06-24T00:40:54.970+0000",
  "type": "item"
}
```

Exposed links:
* bitstreams: list of bitstreams within the item
* owningCollection: the collection where the item belong to
* mappedCollections: the collections where the item is mapped to
* templateItemOf: the collection that have the item as template
 
## Creating an archived item

**POST /api/core/items?owningCollection:<:uuid>**

Administrators can directly create an archived item (bypassing the workflow). An example JSON can be seen below:

```
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

## Updating item metadata

**PUT /api/core/items/<:uuid>**

Provide updated metadata information for an item, when the update is completed the updated object will be returned. The JSON to update can be found below.

```
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

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To withdraw an item, `curl --data '{[ { "op": "replace", "path": "/withdrawn", "value": true}]}' -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The withdraw operation also requires an Authorization header.

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

To reinstate an item, `curl --data '{[ { "op": "replace", "path": "/withdrawn", "value": false}]}' -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The reinstate operation also requires an Authorization header.

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
### Bitstreams
**/api/core/items/<:uuid>/bitstreams**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bitstreams>

It returns the bitstreams within this item. See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

The supported parameters are:
* page, size [see pagination](README.md#Pagination)

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
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist
* 422 Unprocessable Entity - if the collection doesn't exist or the data cannot be resolved to a collection

### Mapped Collections
**/api/core/items/<:uuid>/mappedCollections**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections>

It returns the collections where the item is mapped to. The owningCollection is NOT included in such list.

#### POST operation
To map the item to one or multiple collections a POST request using the text/uri-list mime-type can be used. Each URI goes on a different line

 Example
 ```
 curl -i -X POST https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections
   -H "Content-Type:text/uri-list" -d "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
 ``` 
 
  It adds the collection with uuid `1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb` to the list of collections where the item is mapped to
 
  Return codes:
 * 204: if the mapping succeed (including the case of no-op if the mapping was already in place) 
 * 401 Forbidden - if you are not authenticated
 * 403 Unauthorized - if you are not logged in with sufficient permissions 
 * 405: if the item is a template item
 * 422: if the specified collection or one of the specified collections is not found or is the owningCollection of the item

#### PUT operation
To completely override the collections where the item is mapped to a PUT request using the text/uri-list mime-type can be used
 Example
 ```
 curl -i -X PUT https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections
   -H "Content-Type:text/uri-list" -d "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb"
 ``` 
 
  It will make the collection with uuid `1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb` the only collection where the item is mapped to
 
  Return codes:
 * 204: if the update succeed (including the case of no-op if the mapping was already as requested) 
 * 401 Forbidden - if you are not authenticated
 * 403 Unauthorized - if you are not logged in with sufficient permissions 
 * 405: if the item is a template item
 * 422: if the specified collection or one of the specified collections is not found or is the owningCollection of the item
  
#### DELETE operations
To remove any mapping for the item a DELETE request can be used.

 Example
 ```
 curl -i -X DELETE https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections
 ``` 
 
It is also possible to remove a single mapping using the URL
**/api/core/items/<:uuid>/mappedCollections/<:collectionUUID>**

Example
 ```
 curl -i -X DELETE https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/mappedCollections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb
 ``` 
 
  It will remove the mapping (if any) to the collection with uuid `1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb` for the item `1911e8a4-6939-490c-b58b-a5d70f8d91fb`
 
  Return codes:
 * 204: if the delete succeed (including the case of no-op if the mapping was not set) 
 * 401 Forbidden - if you are not authenticated
 * 403 Unauthorized - if you are not logged in with sufficient permissions 
 * 405: if the item is a template item
 * 422: if the specified collection or one of the specified collections is not found or is the owningCollection of the item

### Template Item
**/api/core/items/<:uuid>/templateItemOf**

Example: to be provided

It returns the collection that have the item as template

## Deleting an item

**DELETE /api/core/items/<:uuid>**

Delete an item.

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist (or was already deleted)
