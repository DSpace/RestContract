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
  "metadata": [
    {
      "key": "dc.contributor.author",
      "value": "Stvilia, Besiki",
      "language": "en"
    },
    {
      "key": "dc.contributor.author",
      "value": "Lee, Dong Joon",
      "language": "en"
    },
    {
      "key": "dc.title",
      "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
      "language": "en"
    },
    {
      "key": "dc.type",
      "value": "Journal Article",
      "language": "en"
    },
    {
      "key": "dc.identifier.url",
      "value": "http://europepmc.org/abstract/MED/28301533",
      "language": "en"
    }
  ],
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
* templateItemOf: the collection that have the item as template
 
## Creating an archived item

**POST /api/core/items**

Administrators can directly create an archived item (bypassing the workflow). An example JSON can be seen below:

```
{
  "name": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
  "metadata": [
    {
      "key": "dc.contributor.author",
      "value": "Stvilia, Besiki",
      "language": "en"
    },
    {
      "key": "dc.title",
      "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
      "language": "en"
    },
    {
      "key": "dc.type",
      "value": "Journal Article",
      "language": "en"
    }
  ],
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
  "metadata": [
    {
      "key": "dc.contributor.author",
      "value": "Velasco, Mercedes",
      "language": "en"
    },
    {
      "key": "dc.title",
      "value": "Test new title",
      "language": "pt_BR"
    }
  ],
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "owningCollectionUuid": "138ac7b0-021a-4dc1-ab9e-6b8c516e1636",
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

To make an item private (or discoverable), `curl --data '{[ { "op": "replace", "path": "/discoverable", "value": false}]}' -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The discoverable operation also requires an Authorization header.


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

### Template Item
**/api/core/items/<:uuid>/templateItemOf**

Example: to be provided

It returns the collection that have the item as template

## Deleting a collection

**DELETE /api/core/items/<:uuid>**

Delete an item.

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist (or was already deleted)