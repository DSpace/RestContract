# ItemExportFormats Endpoints
[Back to the list of all defined endpoints](endpoints.md)

An item export format describes a supported way to export items of a given entity type with the allowed molteplicities (single, multi, single and multi).

## Main Endpoint
**/api/integration/itemexportformat**

Provide access to all the supported formats. The full JSON response document is as follow

```json
{
  "_embedded": {
    "itemexportformats": [
      {
        "id": "endnote",
        "mimeType": "application/x-endnote-refer; charset=UTF-8",
        "entityType": "Publication",
        "molteplicity": "SINGLE",
        "type": "itemexportformat",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/endnote"
          }
        }
      },
      {
        "id": "bibtex",
        "mimeType": "application/x-bibtex; charset=UTF-8",
        "entityType": "Publication",
        "molteplicity": "SINGLE",
        "type": "itemexportformat",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/bibtex"
          }
        }
      },
      {
        "id": "publication-xml",
        "mimeType": "application/xml; charset=UTF-8",
        "entityType": "Publication",
        "molteplicity": "SINGLE_AND_MULTIPLE",
        "type": "itemexportformat",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/publication-xml"
          }
        }
      },
      {
        "id": "person-pdf",
        "mimeType": "application/pdf",
        "entityType": "Person",
        "molteplicity": "SINGLE",
        "type": "itemexportformat",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/person-pdf"
          }
        }
      },
      {
        "id": "person-csv",
        "mimeType": "text/csv",
        "entityType": "Person",
        "molteplicity": "MULTIPLE",
        "type": "itemexportformat",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/person-csv"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/integration/itemexportformat"
    },
    "search": {
      "href": "http://localhost:8080/server/api/integration/itemexportformat/search"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 5,
    "totalPages": 1,
    "number": 0
  }
}
```

Return codes:
* 200 OK - if the operation succeed

## Single ItemExportFormats
**/api/integration/itemexportformat/<:string>**

Provide detailed information about a specific item export format. An example JSON response document:
```json
{
    "id": "person-pdf",
    "mimeType": "application/pdf",
    "entityType": "Person",
    "molteplicity": "SINGLE",
    "type": "itemexportformat",
    "_links": {
        "self": {
            "href": "http://localhost:8080/server/api/integration/itemexportformat/person-pdf"
        }
    }
}
```

Return codes:
* 200 OK - if the operation succeed
* 404 Not found - if a format with the given id doesn't exist

### Search methods
#### byEntityTypeAndMolteplicity
**/api/integration/itemexportformat/search/byEntityTypeAndMolteplicity?entityTypeId=<:string>&molteplicity=<:string>**

It returns the list of the supported item export formats for the given entity type and molteplicity.

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* entityTypeId: the entity type to filter on (e.g. `Publication`, `Person`)
* molteplicity: see `org.dspace.content.crosswalk.CrosswalkMode`, can be one of `SINGLE`, `MULTIPLE`, `SINGLE_AND_MULTIPLE`. When omitted or blank it defaults to `SINGLE_AND_MULTIPLE`.

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the molteplicity parameter is invalid
