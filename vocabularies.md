# Controlled Vocabularies Endpoints
[Back to the list of all defined endpoints](endpoints.md)

All the endpoints here described are restricted to authenticated users.

## Main Endpoint
**/api/submission/vocabularies**   

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
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/srsc/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/srsc"
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
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/srsc/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/srsc"
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
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_iso_languages/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_iso_languages"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies"
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
**/api/submission/vocabularies/<:vocabulary-name>**

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
* id: the id of the vocabulary, it is the same than the name. The id must be Alphanumeric, i.e [A-z0-9]
* name: see id
* scrollable: if true mean that it is possible to scroll all the entries in the authority without providing a filter parameter, see [vocabulary entries](vocabularies.md#controlled-vocabulary-entries)
* hierarchical: if true means that the vocabulary expose a tree structure where some entries are parent of others
* preloadLevel: for hierarchical vocabularies express the preference to preload the tree at a specific level of depth (0 only the top nodes are shown, 1 also their children are preloaded and so on)

Exposed links:
* entries: get entries from the controlled vocabulary

## Linked entities
### controlled vocabulary entries
**/api/submission/vocabularies/<:vocabulary-name>/entries**

It returns the entries in the vocabulary in response to either a user input or a scrollable list, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* filter: the terms, keywords or prefix to use to filter the entries
* exact: can be true or false (default if absent). If true force the vocabulary to provide only entries that match exactly with the filter
* entryID: get just those entries related to a specific vocabulary entry ID. Multiple entries may be returned, e.g. if two entries are just different terms/phrases for the same thing. 

It returns the entries suggested by the controlled vocabulary according to the filter or the entryID. 
The filter and entryID parameters are alternatively, exactly one of them must be used except than for scrollable 
vocabularies than can provide free consultation of the entries.

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the filter and entryID parameters are both provided or if the exact parameter is provided with an invalid value or without the filter parameter
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the filter and entryID are both absent and the vocabulary is NOT scrollable

sample for the vocabulary common_types defined via a value pairs in the submission-forms.xml 
/server/api/submission/vocabularies/common_types/entries?size=2 

```json
{
  "_embedded": {
    "entries": [
      {
        "display": "Animation",
        "value": "Animation",
        "type": "vocabularyEntry"
      },
      {
        "display": "Article",
        "value": "Article",
        "type": "vocabularyEntry"
      }]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?page=1&size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?page=10&size=2"
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

extra sample filtering the suggestion with the term Book
/server/api/submission/vocabularies/common_types/entries?size=2&filter=Book

```json
{
  "_embedded": {
    "entries": [
      {
        "display": "Book",
        "value": "Book",
        "type": "vocabularyEntry"
      },
      {
        "display": "Book chapter",
        "value": "Book chapter",
        "type": "vocabularySuggestion"
      }]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?page=0&size=2"
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
/server/api/submission/vocabularies/srsc/entries?filter=Research&size=2

```json
{
  "_embedded": {
    "entries": [
      {
        "display": "Family research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "otherInformation": {
          "id": "VR131402",
          "parent": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work",
          "hasChildren": "false",
          "note": "Familjeforskning"
        },
        "type": "vocabularyEntry",
        "_links": {
          "vocabularyEntryDetail": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularyEntryDetails/srsc:VR131402"
          }
        }
      },
      {
        "display": "Youth research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Youth research",
        "otherInformation": {
          "id": "VR131403",
          "parent": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work",
          "hasChildren": "false",
          "note": "Ungdomsforskning"
        },
        "type": "vocabularyEntry",
        "_links": {
          "vocabularyEntryDetail": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularyEntryDetails/srsc:VR131403"
          }
        }
      }
    ]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?filter=Research&size=2&page=0"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?filter=Research&size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?filter=Research&size=2&page=1"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/common_types/entries?filter=Research&size=2&page=12"
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

sample for a vocabulary providing an authority value
/server/api/submission/vocabularies/SolrAuthorAuthority/entries?size=1&filter=Bollini

```json
{
  "_embedded": {
    "entries": [
      {
        "authority": "42768ba6-0ba1-4aa3-971e-9c4e27fd7558",
        "display": "Bollini, Andrea",
        "value": "Bollini, Andrea",
        "type": "vocabularyEntry",
        "_links": {
          "vocabularyEntryDetail": {
            "href": "https://dspace7.4science.cloud/server/api/submission/vocabularyEntryDetails/SolrAuthorAuthority:42768ba6-0ba1-4aa3-971e-9c4e27fd7558"
          }
        }
      }]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/SolrAuthorAuthority/entries?page=0&size=1"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/SolrAuthorAuthority/entries?size=1"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/submission/vocabularies/SolrAuthorAuthority/entries?page=0&size=1"
    }
  },
  "page": {
    "size": 1,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```


## Search methods
### Get controlled vocabulary by metadata and collection
**/api/submission/vocabularies/search/byMetadataAndCollection?metadata=<schema.element[.qualifier]>&collection=<:collection-uuid>**

Return the controlled vocabulary configured for the specified metadata and collection if any.

The response is the same than the one of a single controlled vocabulary or an empty body if no controlled vocabulary is available for the specified parameter.

Return codes:
* 200 OK - if the operation succeed and a controlled vocabulary is available
* 204 No content - if the operation succeed but NO controlled vocabulary is available for the specified metadata and collection
* 400 Bad Request - if the metadata or collection parameter are missing of syntactically invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the specified metadata or collection don't exist