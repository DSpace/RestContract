# Metadata Schemas Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/metadataschemas**   

Provide access to the metadata schemas defined in the registry (DBMS based). It returns the list of existent metadata schemas.

Example: <https://demo.dspace.org/server/#/server/api/core/metadataschemas>

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
      "href": "https://demo.dspace.org/server/api/core/metadataschemas/1"
    }
  }
}
```

Exposed links: none

Return codes:
* **200 OK** - if the operation succeed
* **404 Not Found** - if the metadata schema doesn't exist

## Creating a Metadata Schema

**POST /api/core/metadataschemas**

To create a metadata schema, perform a post with the JSON below when logged in as admin.

```json
{
  "prefix": "example",
  "namespace": "http://dublincore.org/documents/example/"
}
```

Return codes:
* **201 Created** - if the operation succeed
* **400 Bad Request** - if the body of the request can't be parsed (for example because of a missing field)
* **401 Unauthorized** - if you are not authenticated
* **403 Forbidden** - if you are not logged in with sufficient permissions
* **422 Unprocessable Entity** - if the request is well-formed, but is invalid based on the given data.
  * If you attempt to create a schema with an empty `prefix` or if it contains dots, commas or spaces or if it's longer than 32 characters.
  * If you attempt to create a schema with an empty `namespace`.

## Updating a Metadata Schema

**PUT /api/core/metadataschemas/<:id>**

Provide updated information about a specific metadata schema, when the update is completed the updated object will be returned. The JSON to update can be found below.
```json
{
  "id": 7,
  "prefix": "example",
  "namespace": "http://dublincore.org/documents/exampleUpdated/"
}
```

Return codes:
* **200 OK** - if the operation succeed
* **400 Bad Request** - if the body of the request can't be parsed (for example because of a missing field)
* **401 Unauthorized** - if you are not authenticated
* **403 Forbidden** - if you are not logged in with sufficient permissions
* **422 Unprocessable Entity** - if the request is well-formed, but is invalid based on the given data.
  * If you attempt to update the `prefix`.
  * If you attempt to update the `namespace` with an empty `namespace`.

## Deleting a Metadata Schema

**DELETE /api/core/metadataschemas/<:id>**

Delete a metadata schema.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the metadata schema doesn't exist (or was already deleted)
