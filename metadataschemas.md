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