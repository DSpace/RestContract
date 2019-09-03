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
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entryValues"
          },
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid"
          }
        }
      },
      {
        "id": "ciencia",
        "name": "ciencia",
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/ciencia/entryValues"
          },
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/ciencia/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/ciencia"
          }
        }
      },
      {
        "id": "my_staff_db",
        "name": "my_staff_db",
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/my_staff_db/entryValues"
          },
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/my_staff_db/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/my_staff_db"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources"
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
**/api/integration/externalsources/<:authority-name>**

Provide detailed information about a specific external source. The JSON response document is as follow
```json
{
  "id": "orcid",
  "name": "orcid",
  "hierarchical": false,
  "type": "authority"
}
```

Exposed links:
* entries: the list of values managed by the external source
* entryValues: the endpoint to retrieve a single value

## Linked entities
### external source entries
**/api/integration/externalsources/<:authority-name>/entries**

It returns the filtered entries managed by the externally, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination) if supported by the external source
* metadata: the metadata field for which the authority is used: mandatory
* query: the terms, keywords or prefix to search: mandatory
* parent: the key of the parent authority when searching in a hierarchical authority 

It returns the entries in the external source matching the query

sample for an external source /server/api/integration/externalsources/orcid/entries?metadata=dc.contributor.author&query=Smith&size=2 
```json
{
  "_embedded": {
    "externalSourceEntries": [
      {
        "id": "Smith, Dean",
        "display": "Smith, Dean",
        "value": "Smith, Dean",
        "metadata": {
            "dc.identifier.orcid": "0000-0002-4271-0436",
            "dc.identifier.uri": "https://orcid.org/0000-0002-4271-0436",
            "dc.contributor.other": "University of Texas Southwestern Medical Center: TX, TX, US",
            "person.familyName": "Smith",
            "person.givenName": "Dean"
        },
        "type": "externalSource",
        "_links": {
          "authority": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/authorities/authors/entryValues/d4b5ca88-9d6d-4a87-b905-fef0f8cae26c"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
          }
        }
      },
      {
        "id": "Smith, Charles",
        "display": "Smith, Charles",
        "value": "Smith, Charles",
        "metadata": {
            "dc.identifier.orcid": "0000-0003-3681-2038",
            "dc.identifier.uri": "https://orcid.org/0000-0003-3681-2038",
            "dc.contributor.other": "University of Mississippi: University, MS, US",
            "person.familyName": "Smith",
            "person.givenName": "Charles"
        },
        "type": "externalSource",
        "_links": {
          "entity": {
            "href": "https://dspace7-internal.atmire.com/server/api/core/item/6fd90bf5-b84f-47b3-aaec-a55bde3a2a5a"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entryValues/0000-0003-3681-2038"
          }
        }
      }
    ]
  }
}
```

### single entry
**GET /api/integration/externalsources/<:authority-name>/entryValues/<:entry-id>**

It returns the data from one entry in an external source

sample for an external source /api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436 
```json
{
  "id": "Smith, Dean",
  "display": "Smith, Dean",
  "value": "Smith, Dean",
  "metadata": {
      "dc.identifier.orcid": "0000-0002-4271-0436",
      "dc.identifier.uri": "https://orcid.org/0000-0002-4271-0436",
      "dc.contributor.other": "University of Texas Southwestern Medical Center: TX, TX, US",
      "person.familyName": "Smith",
      "person.givenName": "Dean"
  },
  "type": "externalSource",
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
    }
  }
}
```

**POST /api/integration/externalsources/<:authority-name>/entryValues/<:entry-id>/authority**

It creates an authority records from the external source 

sample for an external source /api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436/authority
```json
{
  "id": "Smith, Dean",
  "display": "Smith, Dean",
  "value": "Smith, Dean",
  "metadata": {
      "dc.identifier.orcid": "0000-0002-4271-0436",
      "dc.identifier.uri": "https://orcid.org/0000-0002-4271-0436",
      "dc.contributor.other": "University of Texas Southwestern Medical Center: TX, TX, US",
      "person.familyName": "Smith",
      "person.givenName": "Dean"
  },
  "type": "authority",
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/authorities/authors/entries/d4b5ca88-9d6d-4a87-b905-fef0f8cae26c"
    },
    "externalsource": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
    }
  }
}
```

**POST /api/integration/externalsources/<:authority-name>/entryValues/<:entry-id>/entity**

It creates an entity from the external source 

sample for an external source /api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436/entity
```json
{
  "uuid": "83914286-666b-450c-9c42-0d276b30c2f2",
  "name": "Smith, Dean",
  "handle": "10673/20",
  "metadata": {
    "dc.identifier.orcid": [
      {
        "value": "0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "dc.identifier.uri": [
      {
        "value": "https://orcid.org/0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "dc.contributor.other": [
      {
        "value": "University of Texas Southwestern Medical Center: TX, TX, US",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "person.familyName": [
      {
        "value": "Smith",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ],
    "person.givenName": [
      {
        "value": "Dean",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2019-09-02T00:40:54.970+0000",
  "type": "item"
}
```
