# External sources Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/externalsources**   

Provide access to the configured external sources. It returns the list of existent external sources.

Example:
```json
{
  "_embedded": {
    "externalsources": [
      {
        "id": "orcid",
        "name": "orcid",
        "hierarchical": false,
        "type": "externalsource",
        "_links": {
          "entries": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid/entries"
          },
          "entityTypes": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid/entityTypes"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid"
          }
        }
      },
      {
        "id": "ciencia",
        "name": "ciencia",
        "hierarchical": false,
        "type": "externalsource",
        "_links": {
          "entries": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/ciencia/entries"
          },
          "entityTypes": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/ciencia/entityTypes"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/ciencia"
          }
        }
      },
      {
        "id": "my_staff_db",
        "name": "my_staff_db",
        "hierarchical": false,
        "type": "externalsource",
        "_links": {
          "entries": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/my_staff_db/entries"
          },
          "entityTypes": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/my_staff_db/entityTypes"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/my_staff_db"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/integration/externalsources"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 3,
    "totalPages": 1,
    "number": 0
  }
}
```

## Single Authority
**/api/integration/externalsources/<:source-name>**

Provide detailed information about a specific external source. The JSON response document is as follow
```json
{
  "id": "orcid",
  "name": "orcid",
  "hierarchical": false,
  "type": "externalsource"
}
```

Exposed links:
* entries: the list of values managed by the external source
* entityTypes: the list of entity types that are compatible with the provider

## Linked entities
### external source entries
**/api/integration/externalsources/<:source-name>/entries**

It returns the filtered entries managed by the externally, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination) if supported by the external source
* query: the terms, keywords or prefix to search: mandatory
* parent: the key of the parent authority when searching in a hierarchical authority 

It returns the entries in the external source matching the query

sample for an external source /server/api/integration/externalsources/orcid/entries?query=Smith&size=2 
```json
{
  "_embedded": {
    "externalSourceEntries": [
      {
        "id": "0000-0002-4271-0436",
        "display": "Smith, Dean",
        "value": "Smith, Dean",
        "metadata": {
            "dc.identifier.orcid": [
              {
                "value": "0000-0002-4271-0436",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "dc.identifier.uri": [
              {
                "value": "https://orcid.org/0000-0002-4271-0436",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.familyName": [
              {
                "value": "Smith",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.givenName": [
              {
                "value": "Dean",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ]
        },
        "type": "externalSourceEntry",
        "_links": {
          "authority": {
            "href": "https://demo.dspace.org/server/api/integration/authorities/authors/entryValues/d4b5ca88-9d6d-4a87-b905-fef0f8cae26c"
          },
          "parent": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
          }
        }
      },
      {
        "id": "0000-0003-3681-2038",
        "display": "Smith, Charles",
        "value": "Smith, Charles",
        "metadata": {
            "dc.identifier.orcid": [
              {
                "value": "0000-0003-3681-2038",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "dc.identifier.uri": [
              {
                "value": "https://orcid.org/0000-0003-3681-2038",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.familyName": [
              {
                "value": "Smith",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.givenName": [
              {
                "value": "Charles",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ]
        },
        "type": "externalSourceEntry",
        "_links": {
          "entity": {
            "href": "https://demo.dspace.org/server/api/core/item/6fd90bf5-b84f-47b3-aaec-a55bde3a2a5a"
          },
          "parent": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid"
          },          
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid/entryValues/0000-0003-3681-2038"
          }
        }
      }
    ]
  }
}
```

### single entry
**GET /api/integration/externalsources/<:source-name>/entryValues/<:entry-id>**

It returns the data from one entry in an external source

sample for an external source /api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436 
```json
{
  "id": "0000-0002-4271-0436",
  "display": "Smith, Dean",
  "value": "Smith, Dean",
  "type": "externalSourceEntry",
  "externalSource": "orcid",
  "metadata": {
    "dc.identifier.orcid": [
      {
        "value": "0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "dc.identifier.uri": [
      {
        "value": "https://orcid.org/0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "person.familyName": [
      {
        "value": "Smith",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "person.givenName": [
      {
        "value": "Dean",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ]
  },
  "_links": {
    "entity": {
      "href": "https://demo.dspace.org/server/api/core/item/00167e74-c027-4984-8564-85c3fe513d45"
    },
    "parent": {
      "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid"
    },    
    "self": {
      "href": "https://demo.dspace.org/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
    }
  }
}
```

### supported entity types
**/api/integration/externalsources/<:source-name>/entityTypes**

Return the list of entity types that are supported by this external provider. For example a Pubmed provider can return "Publication" and "Dataset" as supported entity types assuming that a separate Dataset entity type has been configured in DSpace

### Search methods
#### findByEntityType
**/api/integration/externalsources/search/findByEntityType?entityType=<:type-label>**

The supported parameters are:
* entityType (mandatory) the label of the entity type
* page, size [see pagination](README.md#Pagination)
It returns a list of external sources that support the requested entity type

Return codes:
* 200 OK - if the operation succeed, a 0 size list can be returned
* 400 Bad Request - if the entityType parameter is missing
