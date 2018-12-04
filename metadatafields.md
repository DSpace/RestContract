# Metadata Fields Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/metadatafields**   

Provide access to the metadata fields defined in the registry (DBMS based). It returns the list of existent metadata fields.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/metadatafields>

## Single Metadata Field
**/api/core/metadatafields/<:id>**

Provide detailed information about a specific metadata schema. The JSON response document is as follow
```json
{
  {
  "id": 8,
  "element": "contributor",
  "qualifier": "advisor",
  "scopeNote": "Use primarily for thesis advisor.",
  "type": "metadatafield",
   "_links": {...},
   "_embedded": {...}
}
```

Exposed links:
* schema: the metadata schema to which the item belongs 

## Linked entities
### Schema
**/api/core/metadatafields/<:id>/schema**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/metadatafields/8/schema>

It returns the metadata schema which the metadata field belongs. See the Metadata Schema endpoint for more info](metadataschemas.md#Single Metadata Schema)


### Search methods
#### bySchema
**/api/core/metadatafields/search/bySchema?schema=<:prefix>**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/metadatafields/search/bySchema?schema=dc>

The supported parameters are:
* **(mandatory)** schema, the prefix of the metadata schema (i.e. "dc", "dcterms", "eperson, etc.)
* page, size [see pagination](README.md#Pagination)