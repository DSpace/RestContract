# Vocabulary Entries Endpoints
[Back to the list of all defined endpoints](endpoints.md)

All the endpoints here are accessible by anonoymous users.

## Main Endpoint
**/api/submission/vocabularyEntryDetails**   

Not allowed. Only subset of vocabulary entries can be retrieved using specific filters see below

## Single Authority
**/api/submission/vocabularyEntryDetails/<:vocabulary-name>:<:entry-id>**

Provide detailed information about a specific vocabulary entry. The JSON response document is as follow
```json
{
      "id" : "srsc:SCB110",
      "value" : "Research Subject Categories::HUMANITIES and RELIGION::Religion/Theology",
      "display" : "Religion/Theology",
      "selectable" : true,
      "otherInformation" : {
          "id": "SCB110",
          "note" : "Religionsvetenskap/Teologi",
          "parent": "HUMANITIES and RELIGION",
          "hasChildren": "true"
      },
      "type" : "vocabularyEntryDetail",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB110"
        },
        "vocabulary" : {
          "href" : "http://localhost/api/submission/vocabularies/srsc"
        },
        "parent" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB110/parent"
        },
        "children" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB110/children"
        },
      }
    }
```

Attributes:
* id: the unique id of the entry
* value: the preferred value to store for the entry
* display: the preferred value to display for the entry
* selectable: in an hierachical vocabulary it could be required to allow the selection only of leaves in the tree 
* otherInformation: an map of additional string attributes useful to provide more context in the UI for the selection

Exposed links:
* vocabulary: the vocabulary that the entry belong to
* parent: for hierarchical vocabulary, the detail entry parent of the current detail entry, if any
* children: for hierarchical vocabulary, the list of children detail entries, if any

Return codes:
* 200 OK - if the operation succeed
* 404 Not Found - if the vocabulary entry is missing or the resource ID is malformed
* 401 Unauthorized - if you are not authenticated

## Search methods
### top
**/api/submission/vocabularyEntryDetails/search/top?vocabulary=<:vocabulary-id>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* vocabulary: the id of the vocabulary to query. It must be an hierarchical vocabulary

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the vocabulary parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the specified vocabulary exists but it is not an hierarchical vocabulary

Provide detailed information about all top entries of a specific hierarchical vocabulary. The JSON response document is as follow

-- this one represent top entries of "srsc" vocabulary
```json, 
{
      "id" : "srsc:SCB11",
      "value" : "HUMANITIES and RELIGION",
      "selectable" : true,
      "otherInformation" : {
          "id": "SCB11",
          "note" : "HUMANIORA och RELIGIONSVETENSKAP",
          "hasChildren": "true"
      },
      "type" : "vocabularyEntryDetail",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB11"
        },
        "vocabulary" : {
          "href" : "http://localhost/api/submission/vocabularies/srsc"
        },
        "parent" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB11/parent"
        },
        "children" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB11/children"
        },
      }
    }, {
      "id" : "srsc:SCB12",
      "value" : "LAW/JURISPRUDENCE",
      "selectable" : true,
      "otherInformation" : {
        "id" : "SCB12",
        "note" : "RÄTTSVETENSKAP/JURIDIK",
        "hasChildren" : "true"
      },
      "type" : "vocabularyEntryDetail",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB12"
        },
        "vocabulary" : {
          "href" : "http://localhost/api/submission/vocabularies/srsc"
        },
        "parent" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB12/parent"
        },
        "children" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB12/children"
        },
      }
    }, 
    ..., 
    {
      "id" : "srsc:SCB23",
      "value" : "INTERDISCIPLINARY RESEARCH AREAS",
      "selectable" : true,
      "otherInformation" : {
        "id" : "SCB23",
        "note" : "TVÄRVETENSKAPLIGA FORSKNINGSOMRÅDEN",
        "hasChildren" : "true"
      },
      "type" : "vocabularyEntryDetail",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB23"
        },
        "vocabulary" : {
          "href" : "http://localhost/api/submission/vocabularies/srsc"
        },
        "parent" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB23/parent"
        },
        "children" : {
          "href" : "http://localhost/api/submission/vocabularyEntryDetails/srsc:SCB23/children"
        },
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/submission/vocabularyEntryDetails/search/top?vocabulary=srsc"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 12,
    "totalPages" : 1,
    "number" : 0
  }
```
