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

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the email parameter is missing or invalid

## Creating a Metadata Field

**POST /api/core/metadatafields?schemaId=<:schemaId>**

To create a metadata field, perform a post with the JSON below when logged in as admin.

```
{
  "element": "contributor",
  "qualifier": "tester",
  "scopeNote": "An agent which provided illustrations for the resource",
  "type": "metadatafield"
}
```

## Updating a Metadata Field

**PUT /api/core/metadatafields/<:id>**

Provide updated information about a specific metadata field, when the update is completed the updated object will be returned. The JSON to update can be found below.
```
{
  "id": 7,
  "element": "coverageUpdated",
  "qualifier": "spatialUpdated",
  "scopeNote": null,
  "type": "metadatafield"
}
```  

## Deleting a Metadata Field

**DELETE /api/core/metadatafields/<:id>**

Delete a metadata field.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the metadata field doesn't exist (or was already deleted)