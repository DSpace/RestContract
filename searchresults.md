# SearchResults Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main endpoint

**/api/discover/searchresults**

This endpoint is unsupported.

## Single SearchResult

**/api/discover/searchresults/<:>**

This endpoint is unsupported.

## Search methods

### searchObjects

**/api/discover/searchresults/search/objects**

This endpoint returns a list of DSpace Objects that match the given parameters. Those parameters are:

- `query`: The discovery search string to will be used to match records. Please note that this will be treated as a Solr/lucene query, which means Solr query syntax is available. However, any special characters must be URL or percent encoded (i.e. %xx).
- `dsoType`: Limit the search to a specific DSpace Object type:
    - all: Execute a query over all available DSO types.
    - item: Search within DSpace items.
    - community: Search within DSpace communities.
    - collection: Search within DSpace collections.
- `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
- `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, then this parameter will be ignored.
- `hitHighlighting`: Whether hit highlighting results should be returned by this request.
- `f.<:filter-name>=<:filter-value>,<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by the DiscoveryConfigurations endpoint (see [here](discoveryconfigurations.md)). For example `f.author=5df05073-3be7-410d-8166-e254369e4166,authority` or `f.title=rainbows,notcontains`. If the filter operator is absent or invalid a "422 Unprocessable Entity" will be returned.
- `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name must match a value returned by the DiscoveryConfigurations endpoint (see [here](discoveryconfigurations.md)). or *default*, followed by a comma and the order direction. For example `sort=default,asc` or `sort=dateissued,desc`.

Sample JSON:

```
{
  "_embedded": {
    "objects": [
      {
        "hitHighlights": {
          "dc.title": [
            "<em>Test</em> item 1"
          ]
        },
        "type": "discover",
        "_links": {
          "indexableObject": {
            "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024"
          }
        },
        "_embedded": {
          "indexableObject": {
            "id": "011e97af-a10b-49ad-a7b6-201b331e1024",
            "uuid": "011e97af-a10b-49ad-a7b6-201b331e1024",
            "name": "Test item 1",
            "handle": "123456789/26",
            "metadata": {
              "dc.contributor.author": [
                {
                  "value": "Smith, John",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.date.accessioned": [
                {
                  "value": "2025-11-12T13:52:57Z",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.identifier": [
                {
                  "value": "test",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.identifier.uri": [
                {
                  "value": "http://localhost:4000/handle/123456789/26",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.title": [
                {
                  "value": "Test item 1",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dspace.entity.type": [
                {
                  "value": "Publication",
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
            "lastModified": "2026-01-30T07:42:06.147410Z",
            "entityType": "Publication",
            "type": "item",
            "_links": {
              "accessStatus": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/accessStatus"
              },
              "bundles": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/bundles"
              },
              "identifiers": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/identifiers"
              },
              "mappedCollections": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/mappedCollections"
              },
              "owningCollection": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/owningCollection"
              },
              "relationships": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/relationships"
              },
              "version": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/version"
              },
              "templateItemOf": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/templateItemOf"
              },
              "thumbnail": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/thumbnail"
              },
              "submitter": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024/submitter"
              },
              "self": {
                "href": "http://localhost:8080/server/api/core/items/011e97af-a10b-49ad-a7b6-201b331e1024"
              }
            }
          }
        }
      },
      {
        "hitHighlights": {
          "dc.title": [
            "<em>Test</em> Item"
          ]
        },
        "type": "discover",
        "_links": {
          "indexableObject": {
            "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198"
          }
        },
        "_embedded": {
          "indexableObject": {
            "id": "14877d8f-243c-4828-ac86-69a57ff4e198",
            "uuid": "14877d8f-243c-4828-ac86-69a57ff4e198",
            "name": "Test Item",
            "handle": "123456789/31",
            "metadata": {
              "dc.contributor.author": [
                {
                  "value": "Smith, John",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                },
                {
                  "value": "Doe, Jane",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 1
                }
              ],
              "dc.date.accessioned": [
                {
                  "value": "2026-03-02T08:29:46Z",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.date.issued": [
                {
                  "value": "2026",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.identifier.uri": [
                {
                  "value": "http://localhost:4000/handle/123456789/31",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.title": [
                {
                  "value": "Test Item",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ],
              "dc.type": [
                {
                  "value": "Book",
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
            "lastModified": "2026-03-02T08:29:46.784203Z",
            "entityType": null,
            "type": "item",
            "_links": {
              "accessStatus": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/accessStatus"
              },
              "bundles": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/bundles"
              },
              "identifiers": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/identifiers"
              },
              "mappedCollections": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/mappedCollections"
              },
              "owningCollection": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/owningCollection"
              },
              "relationships": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/relationships"
              },
              "version": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/version"
              },
              "templateItemOf": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/templateItemOf"
              },
              "thumbnail": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/thumbnail"
              },
              "submitter": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198/submitter"
              },
              "self": {
                "href": "http://localhost:8080/server/api/core/items/14877d8f-243c-4828-ac86-69a57ff4e198"
              }
            }
          }
        }
      }
    ]
  },
  "_links": {
    "first": {
      "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&dsoType=item&page=0&size=2"
    },
    "self": {
      "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&dsoType=item&page=0&size=2"
    },
    "next": {
      "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&dsoType=item&page=1&size=2"
    },
    "last": {
      "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&dsoType=item&page=10&size=2"
    }
  },
  "scope": null,
  "query": "test",
  "appliedFilters": null,
  "sort": null,
  "configuration": null,
  "type": "searchresult",
  "page": {
    "size": 2,
    "totalElements": 22,
    "totalPages": 11,
    "number": 0
  }
}
```
