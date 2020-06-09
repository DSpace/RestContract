# Controlled Vocabularies Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/vocabularies**   

Provide access to the configured controlled vocabularies. It returns the list of existent controlled vocabularies.

Example:
```json
{
  "_embedded": {
    "vocabularies": [
      {
        "id": "srsc",
        "name": "srsc",
        "scrollable": false,
        "hierarchical": true,
        "preloadLevel": 1,
        "type": "vocabulary",
        "_links": {
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/srsc/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/srsc"
          }
        }
      },
      {
        "id": "common_types",
        "name": "common_types",
        "scrollable": true,
        "hierarchical": false,
        "type": "vocabulary",
        "_links": {
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types"
          }
        }
      },
      {
        "id": "common_iso_languages",
        "name": "common_iso_languages",
        "scrollable": true,
        "hierarchical": false,
        "type": "vocabulary",
        "_links": {
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_iso_languages/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_iso_languages"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies"
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

## Single Vocabulary
**/api/integration/vocabularies/<:vocabulary-name>**

Provide detailed information about a specific controlled vocabulary. The JSON response document is as follow
```json
{
  "id": "srsc",
  "name": "srsc",
  "scrollable": false,
  "hierarchical": true,
  "preloadLevel": 1,
  "type": "vocabulary"
}
```

Attributes
* id: the id of the vocabulary, it is the same than the name
* name: see id
* scrollable: if true mean that it is possible to scroll all the entries in the authority without providing a query parameter, see (vocabulary entries)[vocabularies.md#vocabulary-entries]
* hierarchical: if true means that the vocabulary expose a tree structure where some entries are parent of others
* preloadLevel: for hierarchical vocabularies express the preference to preload the tree at a specific level of depth (0 only the top nodes are shown, 1 also their childrens are preloaded and so on)

Exposed links:
* entries: get entries from the controlled vocabulary

## Linked entities
### controlled vocabulary entries
**/api/integration/vocabularies/<:vocabulary-name>/entries**

It returns the entries suggested by the vocabulary in response to the user query or a scrollable list, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* metadata: the metadata field for which the authority is used: mandatory
* collection: the uuid of the collection where the item belong to: mandatory
* query: the terms, keywords or prefix to search
* exact: can be true or false (default if absent). If true force the vocabulary to provide only entries that match exactly with the query
* entryID: get the suggestions related to a specific vocabulary entry 
* collection: the uuid of the collection where the item belong to

It returns the entries suggested by the controlled vocabulary according to the query or the entryID. 
The query and entryID parameters are alternatively, exactly one of them must be used except than for scrollable 
vocabularies than can provide free suggestions.

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the metadata or collection parameters are missing or if the query and entryID parameters are both provided or if the exact parameter is provided with an invalid value or without the query parameter
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the metadata and collection parameters don't resolve to the queried vocabulary or if the query and entryID are both absent and the vocabulary is NOT scrollable

sample for the vocabulary common_types defined via a value pairs in the submission-forms.xml 
/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&size=2 

```json
{
  "_embedded": {
    "vocabularyEntries": [
      {
        "display": "Animation",
        "value": "Animation",
        "type": "vocabularyEntry"
      },
      {
        "display": "Article",
        "value": "Article",
        "type": "vocabularyEntry"
      }
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&page=1&size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&page=10&size=2"
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

extra sample filtering the suggestion with the query Book
/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&size=2&query=Book

```json
{
  "_embedded": {
    "vocabularyEntries": [
      {
        "display": "Book",
        "value": "Book",
        "type": "vocabularyEntry"
      },
      {
        "display": "Book chapter",
        "value": "Book chapter",
        "type": "vocabularySuggestion"
      }
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.type&page=0&size=2"
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

sample for a hierarchical authority  (srsc): 
/server/api/integration/vocabularies/srsc/entries?metadata=dc.subject&query=Research&size=2

```json
{
  "_embedded": {
    "vocabularyEntries": [
      {
        "display": "Family research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "otherInformation": {
          "id": "VR131402",
          "parent": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work",
          "hasChildrens": "false",
          "note": "Familjeforskning"
        },
        "type": "vocabularyEntry",
        "_links": {
          "vocabularyEntryDetail": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularyEntryDetails/srsc:VR131402"
          }
        }
      },
      {
        "display": "Youth research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Youth research",
        "otherInformation": {
          "id": "VR131403",
          "parent": Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work,
          "hasChildrens": "false",
          "note": "Ungdomsforskning"
        },
        "type": "vocabularyEntry",
        "_links": {
          "vocabularyEntryDetail": {
            "href": "https://dspace7.4science.cloud/server/api/integration/vocabularyEntryDetails/srsc:VR131403"
          }
        }
      }
    ]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.subject&query=Research&size=2&page=0"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.subject&query=Research&size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.subject&query=Research&size=2&page=1"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/integration/vocabularies/common_types/entries?metadata=dc.subject&query=Research&size=2&page=12"
    }
  },
  "page": {
    "size": 2,
    "totalElements": 26,
    "totalPages": 13,
    "number": 0
  }
}
```

## Search methods
### Get controlled vocabulary by metadata and collection
**/api/integration/vocabularies/search/byMetadataAndCollection?metadata=<schema.element[.qualifier]>&collection=<:collection-uuid>**

Return the controlled vocabulary configured for the specified metadata and collection if any.

The response is the same than the one of a single controlled vocabulary or an empty body if no controlled vocabulary is available for the specified parameter.

Return codes:
* 200 OK - if the operation succeed and a controlled vocabulary is available
* 204 No content - if the operation succeed but NO controlled vocabulary is available for the specified metadata and collection
* 400 Bad Request - if the metadata or collection parameter are missing of syntactically invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the specified metadata or collection don't exist