# Authorities Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/authorities**   

Provide access to the configured authorities. It returns the list of existent authorities.

Example:
```json
{
  "_embedded": {
    "authorities": [
      {
        "id": "srsc",
        "name": "srsc",
        "scrollable": false,
        "hierarchical": true,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc"
          }
        }
      },
      {
        "id": "common_types",
        "name": "common_types",
        "scrollable": false,
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types"
          }
        }
      },
      {
        "id": "common_iso_languages",
        "name": "common_iso_languages",
        "scrollable": false,
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities"
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
**/api/integration/authorities/<:authority-name>**

Provide detailed information about a specific authority. The JSON response document is as follow
```json
{
  "id": "srsc",
  "name": "srsc",
  "scrollable": false,
  "hierarchical": true,
  "type": "authority"
}
```

Exposed links:
* entries: the list of values managed by the authority
* entryValues: the endpoint to retrieve a single value

## Linked entities
### authority entries
**/api/integration/authorities/<:authority-name>/entries**

It returns the filtered entries managed by the authority, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* metadata: the metadata field for which the authority is used: mandatory
* query: the terms, keywords or prefix to search: mandatory
* parent: the key of the parent authority when searching in a hierarchical authority 
* collection: the uuid of the collection where the item belong to

It returns the entries in the authority matching the query

TODO: the example below returns too many responses, and doesn't contain Book as the top result

sample for an authority /server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&size=2 
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "Dataset",
        "display": "Dataset",
        "value": "Dataset",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Dataset"
          }
        }
      },
      {
        "id": "Image, 3-D",
        "display": "Image, 3-D",
        "value": "Image, 3-D",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Image%2C%203-D"
          }
        }
      },
      {
        "id": "Book",
        "display": "Book",
        "value": "Book",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Book"
          }
        }
      }
    ]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=1&size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=10&size=2"
    }
  },
  "page": {
    "size": 2,
    "totalElements": 22,
    "totalPages": 11,
    "number": 0
  }
}
```

sample for a hierarchical authority  (srsc): /server/api/integration/authorities/srsc/entries?metadata=dc.subject&query=Research&size=2
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "VR131402",
        "display": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "otherInformation": {
          "parent": "SCB1314",
          "note": "Familjeforskning"
        },
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/VR131402"
          },
          "parent": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/SCB1314"
          }
        }
      },
      {
        "id": "ResearchSubjectCategories",
        "display": "Research Subject Categories",
        "value": "Research Subject Categories",
        "otherInformation": {
          "note": "Ämneskategorier för vetenskapliga publikationer"
        },
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/ResearchSubjectCategories"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entries?metadata=dc.subject&query=Research&size=2"
    }
  },
  "page": {
    "size": 2,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

### authority entry values
**/api/integration/authorities/<:authority-name>/entryValues/<:entry-id>**

It returns the data from one entry in an authority

sample for an authority /server/api/integration/authorities/common_types/entryValues/Book 
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "Book",
        "display": "Book",
        "value": "Book",
        "otherInformation": {},
        "type": "authority"
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```
