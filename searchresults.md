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
  "id": null,
  "scope": null,
  "query": "test",
  "appliedFilters": null,
  "sort": null,
  "configuration": "default",
  "type": "searchresult",
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&configuration=default&dsoType=collection"
    }
  },
  "_embedded": {
    "searchResult": {
      "self": {},
      "_embedded": {
        "objects": [
          {
            "hitHighlights": {
              "dc.title": [
                "<em>Test</em> Collection 3"
              ]
            },
            "type": "discover",
            "_links": {
              "indexableObject": {
                "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d"
              }
            },
            "_embedded": {
              "indexableObject": {
                "id": "d1ad480f-e918-4905-ac12-a7826c46407d",
                "uuid": "d1ad480f-e918-4905-ac12-a7826c46407d",
                "name": "Test Collection 3",
                "handle": "123456789/4",
                "metadata": {
                  "dc.identifier.uri": [
                    {
                      "value": "http://localhost:4000/handle/123456789/4",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ],
                  "dc.title": [
                    {
                      "value": "Test Collection 3",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ]
                },
                "archivedItemsCount": -1,
                "type": "collection",
                "_links": {
                  "harvester": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/harvester"
                  },
                  "itemtemplate": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/itemtemplate"
                  },
                  "license": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/license"
                  },
                  "logo": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/logo"
                  },
                  "mappedItems": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/mappedItems"
                  },
                  "parentCommunity": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/parentCommunity"
                  },
                  "adminGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/adminGroup"
                  },
                  "submittersGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/submittersGroup"
                  },
                  "itemReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/itemReadGroup"
                  },
                  "bitstreamReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/bitstreamReadGroup"
                  },
                  "self": {
                    "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d"
                  },
                  "workflowGroups": [
                    {
                      "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/workflowGroups/reviewer",
                      "name": "reviewer"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/workflowGroups/editor",
                      "name": "editor"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/d1ad480f-e918-4905-ac12-a7826c46407d/workflowGroups/finaleditor",
                      "name": "finaleditor"
                    }
                  ]
                }
              }
            }
          },
          {
            "hitHighlights": {
              "dc.title": [
                "<em>Test</em> Collection 1"
              ]
            },
            "type": "discover",
            "_links": {
              "indexableObject": {
                "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09"
              }
            },
            "_embedded": {
              "indexableObject": {
                "id": "5533a5cf-a197-426c-a23b-7120f7c67a09",
                "uuid": "5533a5cf-a197-426c-a23b-7120f7c67a09",
                "name": "Test Collection 1",
                "handle": "123456789/2",
                "metadata": {
                  "dc.identifier.uri": [
                    {
                      "value": "http://localhost:4000/handle/123456789/2",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ],
                  "dc.title": [
                    {
                      "value": "Test Collection 1",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ]
                },
                "archivedItemsCount": -1,
                "type": "collection",
                "_links": {
                  "harvester": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/harvester"
                  },
                  "itemtemplate": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/itemtemplate"
                  },
                  "license": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/license"
                  },
                  "logo": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/logo"
                  },
                  "mappedItems": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/mappedItems"
                  },
                  "parentCommunity": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/parentCommunity"
                  },
                  "adminGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/adminGroup"
                  },
                  "submittersGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/submittersGroup"
                  },
                  "itemReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/itemReadGroup"
                  },
                  "bitstreamReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/bitstreamReadGroup"
                  },
                  "self": {
                    "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09"
                  },
                  "workflowGroups": [
                    {
                      "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/workflowGroups/reviewer",
                      "name": "reviewer"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/workflowGroups/editor",
                      "name": "editor"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/5533a5cf-a197-426c-a23b-7120f7c67a09/workflowGroups/finaleditor",
                      "name": "finaleditor"
                    }
                  ]
                }
              }
            }
          },
          {
            "hitHighlights": {
              "dc.title": [
                "<em>Test</em> Collection 2"
              ]
            },
            "type": "discover",
            "_links": {
              "indexableObject": {
                "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527"
              }
            },
            "_embedded": {
              "indexableObject": {
                "id": "a5c2d45f-2a8a-4648-a20e-ff0589ae7527",
                "uuid": "a5c2d45f-2a8a-4648-a20e-ff0589ae7527",
                "name": "Test Collection 2",
                "handle": "123456789/3",
                "metadata": {
                  "dc.identifier.uri": [
                    {
                      "value": "http://localhost:4000/handle/123456789/3",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ],
                  "dc.title": [
                    {
                      "value": "Test Collection 2",
                      "language": null,
                      "authority": null,
                      "confidence": -1,
                      "place": 0
                    }
                  ]
                },
                "archivedItemsCount": -1,
                "type": "collection",
                "_links": {
                  "harvester": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/harvester"
                  },
                  "itemtemplate": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/itemtemplate"
                  },
                  "license": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/license"
                  },
                  "logo": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/logo"
                  },
                  "mappedItems": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/mappedItems"
                  },
                  "parentCommunity": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/parentCommunity"
                  },
                  "adminGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/adminGroup"
                  },
                  "submittersGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/submittersGroup"
                  },
                  "itemReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/itemReadGroup"
                  },
                  "bitstreamReadGroup": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/bitstreamReadGroup"
                  },
                  "self": {
                    "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527"
                  },
                  "workflowGroups": [
                    {
                      "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/workflowGroups/reviewer",
                      "name": "reviewer"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/workflowGroups/editor",
                      "name": "editor"
                    },
                    {
                      "href": "http://localhost:8080/server/api/core/collections/a5c2d45f-2a8a-4648-a20e-ff0589ae7527/workflowGroups/finaleditor",
                      "name": "finaleditor"
                    }
                  ]
                }
              }
            }
          }
        ]
      },
      "page": {
        "number": 0,
        "size": 20,
        "totalPages": 1,
        "totalElements": 3
      },
      "_links": {
        "self": {
          "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?query=test&configuration=default&dsoType=collection"
        }
      }
    }
  }
}
```
