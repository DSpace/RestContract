# Metadata Schemas Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/metadataschemas**   

Provide access to the metadata schemas defined in the registry (DBMS based). It returns the list of existent metadata schemas.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/metadataschemas>

## Single Metadata Schema
**/api/core/metadataschemas/<:id>**

Provide detailed information about a specific metadata schema. The JSON response document is as follow
```json
{
  "id": 1,
  "prefix": "dc",
  "namespace": "http://dublincore.org/documents/dcmi-terms/",
  "type": "metadataschema",
  "_links": {
    "self": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/core/metadataschemas/1"
    }
  }
}
```

Exposed links: none

## Creating a Metadata Schema

**POST /api/core/metadataschemas**

To create a metadata schema, perform a post with the JSON below when logged in as admin.

```
{
  "prefix": "example",
  "namespace": "http://dublincore.org/documents/example/",
  "type": "metadataschema"
}
```

## Updating a Metadata Schema

**PUT /api/core/metadataschemas/<:id>**

Provide updated information about a specific metadata schema, when the update is completed the updated object will be returned. The JSON to update can be found below.
```
{
  "id": 7,
  "prefix": "exampleUpdated",
  "namespace": "http://dublincore.org/documents/exampleUpdated/",
  "type": "metadataschema"
}
```  

## Deleting a Metadata Schema

**DELETE /api/core/metadataschemas/<:id>**

Delete a metadata schema.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the metadata schema doesn't exist (or was already deleted)