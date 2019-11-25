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
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid"
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
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/ciencia/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/ciencia"
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
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/my_staff_db/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/my_staff_db"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/externalsources"
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
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/authors/entryValues/d4b5ca88-9d6d-4a87-b905-fef0f8cae26c"
          },
          "parent": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
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
            "href": "https://dspace7.4science.cloud/server/api/core/item/6fd90bf5-b84f-47b3-aaec-a55bde3a2a5a"
          },
          "parent": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid"
          },          
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid/entryValues/0000-0003-3681-2038"
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
      "href": "https://dspace7.4science.cloud/server/api/core/item/00167e74-c027-4984-8564-85c3fe513d45"
    },
    "parent": {
      "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid"
    },    
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
    }
  }
}
```
