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
}```

Exposed links:
* bitstreams: list of bitstreams within the item
* owningCollection: the collection where the item belong to
* templateItemOf: the collection that have the item as template
 

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