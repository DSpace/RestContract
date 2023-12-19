# Metadata Fields Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/metadatafields**   

Provide access to the metadata fields defined in the registry (DBMS based). It returns the list of existent metadata fields.

Example: <https://demo.dspace.org/server/#/server/api/core/metadatafields>

## Single Metadata Field
**/api/core/metadatafields/<:id>**

Provide detailed information about a specific metadata field. The JSON response document is as follows
```json
{
  "id": 8,
  "element": "contributor",
  "qualifier": "advisor",
  "scopeNote": "Use primarily for thesis advisor.",
  "type": "metadatafield"
}
```

Exposed links:
* schema: the metadata schema to which the item belongs 

## Linked entities
### Schema
**/api/core/metadatafields/<:id>/schema**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/metadatafields/8/schema>

It returns the metadata schema which the metadata field belongs. See the Metadata Schema endpoint for more info](metadataschemas.md#Single Metadata Schema)


### Search methods
#### bySchema
**/api/core/metadatafields/search/bySchema?schema=<:prefix>**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/metadatafields/search/bySchema?schema=dc>

The supported parameters are:
* **(mandatory)** schema, the prefix of the metadata schema (i.e. "dc", "dcterms", "eperson, etc.)
* page, size [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeed

#### byFieldName
**/api/core/metadatafields/search/byFieldName**

This endpoint supports the parameters (any combination of parameters is allowed):
* schema, an exact match of the prefix of the metadata schema (e.g. "dc", "dcterms", "eperson")
* element, an exact match of the field's element (e.g. "contributor", "title")
* qualifier, an exact match of the field's qualifier (e.g. "author", "alternative")
* query, part of the fully qualified field, should start with the start of the schema, element or qualifier (e.g. "dc.ti", "contributor", "auth", "contributor.ot")
* exactName, The exact fully qualified field, should use the syntax schema.element.qualifier or schema.element if no qualifier exists (e.g. "dc.title", "dc.contributor.author"). It will only return one value if there's an exact match
* page, size [see pagination](README.md#Pagination)

Examples:
* /api/core/metadatafields/search/byFieldName?schema=dc&query=author
* /api/core/metadatafields/search/byFieldName?query=id&qualifier=uri

Return codes:
* 200 OK - if the operation succeed

## Creating a Metadata Field

**POST /api/core/metadatafields?schemaId=<:schemaId>**

To create a metadata field, perform a post with the JSON below when logged in as admin.

```json
{
  "element": "contributor",
  "qualifier": "tester",
  "scopeNote": "An agent which provided illustrations for the resource"
}
```

Return codes:
* **201 Created** - if the operation succeed
* **400 Bad Request** - if the body of the request can't be parsed (for example because of a missing field)
* **401 Unauthorized** - if you are not authenticated
* **403 Forbidden** - if you are not logged in with sufficient permissions
* **422 Unprocessable Entity** - if the request is well-formed, but is invalid based on the given data.
  * If the schema ID does not exist
  * If you attempt to create a field with an empty `element` or if it contains dots, commas or spaces or if it's longer than 64 characters.
  * If you attempt to create a field with a `qualifier` contains dots, commas or spaces or if it's longer than 64 characters.

## Updating a Metadata Field

**PUT /api/core/metadatafields/<:id>**

To update the `scopeNote` of a specific metadata field, when the update is completed the updated object will be returned. The JSON to update can be found below.
```json
{
  "id": 7,
  "element": "contributor",
  "qualifier": "tester",
  "scopeNote": null
}
```

Return codes:
* **200 OK** - if the operation succeed
* **400 Bad Request** - if the body of the request can't be parsed (for example because of a missing field)
* **401 Unauthorized** - if you are not authenticated
* **403 Forbidden** - if you are not logged in with sufficient permissions
* **404 Not found** - if the metadata field doesn't exist
* **422 Unprocessable Entity** - if the request is well-formed, but is invalid based on the given data.
  * If you attempt to update the `element`.
  * If you attempt to update the `qualifier`.

## Deleting a Metadata Field

**DELETE /api/core/metadatafields/<:id>**

Delete a metadata field.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the metadata field doesn't exist (or was already deleted)
