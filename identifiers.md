# Identifier endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/pid/identifiers**

* GET method not implemented. To fetch an identifier for an item, see (the Items endpoint)[items.md].

**POST /api/pid/identifiers?type=<:identifier_type>**
Creates or registers a new identifier for this item. A 'type' parameter is required to indicate which sort of
identifier to create, eg. doi, handle, or other supported identifier types.
Currently, only DOI registration is supported with this method.

The item to be identified must be supplied as URI in the request body using the text/uri-list content-type.

This operation would typically be used to mint and queue a DOI for registration for an item that is already archived.

   ```
   # Example curl command to mint or register a new identifiers
   curl -i -X POST "http://localhost:8080/rest/api/identifiers?type=doi" 
        -H "Content-Type:text/uri-list" 
        -d "http://localhost:8080/rest/api/core/items/5ad50035-ca22-4a4d-84ca-d5132f34f588"
   ```

On success, the identifier resource is returned as a response.
   ```json
    {
      "value" : "https://doi.org/10.33515/dspace-61",
      "identifierType" : "doi",
      "identifierStatus" : "TO_BE_REGISTERED",
      "type" : "identifier"
    }
   ```

Return codes:
* 201 Created - if the operation succeeds
* 400 Bad Request - if the item id param is missing or invalid (not an uuid), or if the type param is missing or invalid, or if the DOI is already registered
* 401 Unauthorized - if you are not authenticated and versioning is not public
* 403 Forbidden - if you are not logged in with sufficient permissions and versioning is not public
* 404 Not found - if the item doesn't exist

## Get identifiers for item
**GET /api/pid/identifiers/search/findByItem?uuid=<:uuid>

Return an array of identifiers associated with the given item.
```json
{
  "_embedded": {
    "identifiers": [
      {
        "id": null,
        "value": "https://doi.org/10.33515/dspace-68",
        "identifierType": "doi",
        "identifierStatus": "TO_BE_REGISTERED",
        "type": "identifier",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/ppid/identifiers"
          }
        }
      },
      {
        "id": null,
        "value": "http://localhost:4000/handle/123456789/375",
        "identifierType": "handle",
        "identifierStatus": null,
        "type": "identifier",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/ppid/identifiers"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/pid/identifiers/search/findByItem?uuid=71d45ca4-3a50-40d3-b35a-b17cccac3575"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

## Find DSO by identifier endpoint
**GET /api/pid/find?id=<:identifier>**

Resolve a given <:identifier> string to a DSpace Object and redirect the request to the endpoint for that resource.
The identifier string should be a handle or DOI.

Returns:

**302** redirect to the object on success

**404** not found if the object cannot be found

**501** not implemented if the identifier is not resolvable