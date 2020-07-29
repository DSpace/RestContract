# Discovery Search Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Search Endpoint
**/api/discover/search**   

This endpoint provides the functionality of the Discovery search screen: https://wiki.duraspace.org/display/DSDOC6x/Discovery.
It will provide detailed information on the Discovery search configuration that can be used to build complex searches.

It supports the following parameters:
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search will to be limited. If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml`[https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L28] that configuration will be returned. Otherwise the default configuration will be returned.
* `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored. Two special configurations always exists *workspace* and *workflow*. These configuration are used by the MyDSpace functionality, more details at the bottom of this page.

The JSON response document is as follow
```json
{
  "filters": [
    {
      "filter" : "title",
      "hasFacets": false,
      "type": "text",
      "operators": [
        {
          "operator": "equals"
        },
        {
          "operator": "notequals"
        },
        {
          "operator": "authority"
        },
        {
          "operator": "notauthority"
        },
        {
          "operator": "contains"
        },
        {
          "operator": "notcontains"
        }
      ],
      "openByDefault": false
    },
    {
      "filter" : "author",
      "hasFacets": true,
      "type": "text",
      "operators": [
        {
          "operator": "equals"
        },
        {
          "operator": "notequals"
        },
        {
          "operator": "authority"
        },
        {
          "operator": "notauthority"
        },
        {
          "operator": "contains"
        },
        {
          "operator": "notcontains"
        }
      ],
      "openByDefault": true
    },
    {
      "filter": "subject",
      "hasFacets": true,
      "type": "hierarchical",
      "operators": [
        {
          "operator": "equals"
        },
        {
          "operator": "notequals"
        },
        {
          "operator": "authority"
        },
        {
          "operator": "notauthority"
        },
        {
          "operator": "contains"
        },
        {
          "operator": "notcontains"
        }
      ],
      "openByDefault": false
    },
    {
      "filter": "dateIssued",
      "hasFacets": true,
      "type": "date",
      "operators": [
        {
          "operator": "equals"
        },
        {
          "operator": "notequals"
        },
        {
          "operator": "authority"
        },
        {
          "operator": "notauthority"
        },
        {
          "operator": "contains"
        },
        {
          "operator": "notcontains"
        }
      ],
      "openByDefault": false
    },
    {
      "filter": "has_content_in_original_bundle",
      "hasFacets": true,
      "type": "standard",
      "operators": [
        {
          "operator": "equals"
        },
        {
          "operator": "notequals"
        },
        {
          "operator": "authority"
        },
        {
          "operator": "notauthority"
        },
        {
          "operator": "contains"
        },
        {
          "operator": "notcontains"
        }
      ],
      "openByDefault": false
    }
  ],
  "sortOptions": [
    {
      "name": "score"
    },
    {
      "name": "dc.title"
    },
    {
      "name": "dc.date.issued"
    },
    {
      "name": "dc.date.accessioned"
    }
  ],
  "type": "discover"
}
```

* filters: Provides a list of advanced search filters that can be used to limit the result set as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L97
* operators: A list of supported operators that can be combined on each search filter.
* sortOptions: The sort options available for this query type as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L112

Exposed links:
* objects: link to get the actual list of objects that match the search query
* facets: link to get the list of facet values and counts associated with this search query as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L86

### Matching DSpace objects search results
**/api/discover/search/objects**

This endpoint returns a list of DSpace Objects that match the given type. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records. Please note that this will be treat as a lucene/SOLR query so if special characters need to be literally searched escape them. 
* `dsoType`: Limit the search to a specific DSpace Object type:
     * all: Execute a query over all available DSO types.
     * item: Only search the DSpace items.
     * community: Search within the DSpace community records.
     * collection: Limit the search to DSpace collection records.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored.
* `f.<:filter-name>=<:filter-value>,<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `f.author=5df05073-3be7-410d-8166-e254369e4166,authority` or `f.title=rainbows,notcontains`.
* `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name must match a value returned by the parent search endpoint (see above) or *default*, followed by a comma and the order direction. For example `sort=default,asc` or `sort=dateissued,desc`.

Example: TODO

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "appliedFilters": [
      {
        "filter" : "title",
        "operator" : "notcontains",
        "value" : "abcd",
        "label" : "abcd"
      },
      {
        "filter" : "author",
        "operator" : "authority",
        "value" : "1234",
        "label" : "Smith, Donald"
      }
  ],
  "sort" : {
    "by" : "dc.date.issued",
    "order" : "asc"
  },
  "_embedded" : {
    "searchResults": {
      "_embedded": {
        "objects" : [
          {
            "hitHighlights": {
              "dc.description.abstract" : "This is the <em>very cool</em> abstract of this item",
              "dc.publisher" : "My <em>very cool</em> publisher"
            },
            "_links" : {
              "indexableObject" : {
                "href": "/api/core/items/9f3288b2-f2ad-454f-9f4c-70325646dcee"
              }
            },
            "_embedded" : {
              "indexableObject" : {
                "uuid": "9f3288b2-f2ad-454f-9f4c-70325646dcee",
                "name": "Test Webpage",
                "handle": "10673/4"
              }
            }
          },
          {
            "hitHighlights": { },
            "_links" : {
              "indexableObject" : {
                "href": "/api/core/items/ff7ec3a4-0aab-418b-94fc-d0e8189084db"
              }
            },
            "_embedded" : {
              "indexableObject" : {
                "uuid": "ff7ec3a4-0aab-418b-94fc-d0e8189084db",
                "name": "Test Item with no hit highlights",
                "handle": "10673/5"
              }
            }
          }
        ]
      },
      "_links": {
        "first": {
          "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=0&size=5"
        },
        "self": {
          "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=0&size=5"
        },
        "next": {
          "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=1&size=5"
        },
        "last": {
          "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=2&size=5"
        }
      },
      "page": {
        "number": 0,
        "size": 20,
        "totalElements": 12,
        "totalPages": 3
      }
    },
    "facets": [
      {
        "name" : "author",
        "facetType": "text",
        "facetLimit": 10,
        "_embedded" : {
          "values" : [
            {
              "value" : "Smith, Donald 2",
              "count" : 100,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+2,equals"
              }
            },
            {
              "value" : "Smith, Donald 1",
              "count" : 80,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+1,equals"
              }
            },
            {
              "value" : "Smith, Donald 3",
              "count" : 10,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+3,equals"
              }
            }
          ]
        },
        "_links": {
          "self": {
            "href": "/api/discover/facets/author?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        }
      },{
        "name" : "subject",
        "facetType": "hierarchical",
        "facetLimit": 10,
        "_embedded" : {
          "values" : [
            {
              "value" : "Java",
              "count" : 100,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=Java"
              }
            },
            {
              "value" : "SQL",
              "count" : 80,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=SQL"
              }
            },
            {
              "value" : "CSS",
              "count" : 10,
              "_links": {
                "search" : "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=CSS"
              }
            }
          ]
        },
        "_links": {
          "self": {
            "href": "/api/discover/facets/subject?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        }
      },{
        "name": "dateIssued",
        "facetType": "date",
        "facetLimit": 10,
        "minValue": "1940-03-15",
        "maxValue": "2017-11-06",
        "_links": {
          "self": {
            "href": "/api/discover/facets/dateIssued?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        },
        "page": {
          "number": 0,
          "size": 10
        },
        "_embedded": {
          "values": [
            {
              "label": "1940 - 1959",
              "count": 23370,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1940 TO 1959],equals"
                }
              }
            },
            {
              "label": "1960 - 1979",
              "count": 45044,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1960 TO 1979],equals"
                }
              }
            },
            {
              "label": "1980 - 1999",
              "count": 56128,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1980 TO 1999],equals"
                }
              }
            },
            {
              "label": "2000 - 2017",
              "count": 51707,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[2000 TO 2017],equals"
                }
              }
            }
          ]
        }
      }
    ]  
  },
  "_links": {
    "self": {
      "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=0&size=5"
    },
    "facets": {
      "href": "/api/discover/search/facets?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
    },
    "objects": {
      "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=0&size=5"
    }
  }
}
```

### Matching Facets search results
**/api/discover/search/facets**

This endpoint returns a list of configured facets with their respective values. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records. Please note that this will be treat as a lucene/SOLR query so if special characters need to be literally searched escape them.
* `dsoType`: Limit the search to a specific or multiple DSpace Object types. This field is repeatable to define multiple types:
     * all: Execute a query over all available DSO types.
     * item: Only search the DSpace items.
     * community: Search within the DSpace community records.
     * collection: Limit the search to DSpace collection records.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored.
* `f.<:filter-name>=<:filter-value>,<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `f.author=5df05073-3be7-410d-8166-e254369e4166,authority` or `f.title=rainbows,notcontains`.
* `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name must match a value returned by the parent search endpoint (see above) or *default*, followed by a comma and the order direction. For example `sort=default,asc` or `sort=dateissued,desc`.

Some facets can be configured in the `discovery.xml` file to expose minimum and maximum values. In the example below the `dateIssued` filter has this configuration enabled.

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "appliedFilters": [
      {
        "filter" : "title",
        "operator" : "notcontains",
        "value" : "abcd",
        "label" : "abcd"
      },
      {
        "filter" : "author",
        "operator" : "authority",
        "value" : "1234",
        "label" : "Smith, Donald"
      }
  ],
  "sort" : {
    "by" : "dc.date.issued",
    "order" : "asc"
  },
  "page": {
    	"size": 5,
    	"totalElements": 14,
    	"totalPages": 3,
    	"number": 0
  },
  "_embedded" : {
    "facets" : [
      {
        "name" : "author",
        "facetType": "text",
        "facetLimit": 5,
        "_links": {
          "next" : {
            "href": "/api/discover/facets/author?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=1&size=5"
          },
          "self" : {
            "href":  "/api/discover/facets/author?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        },
        "page": {
          "number": 0,
          "size": 5
        },
        "_embedded" : {
          "values" : [
              {
                "label" : "Smith, Donald 2",
                "count" : 100,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+2,equals"
                  }
                }
              },
              {
                "label" : "Smith, Donald 1",
                "count" : 80,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+1,equals"
                  }
                }
              },
              {
                "label" : "Smith, Donald 3",
                "count" : 10,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+3,equals"
                  }
                }
              }
          ]
        }
      },
      {
        "name" : "subject",
        "facetType": "text",
        "facetLimit": 5,
        "_links": {
          "next" : {
            "href": "/api/discover/facets/subject?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=1&size=5"
          },
          "self" : {
            "href":  "/api/discover/facets/subject?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        },
        "page": {
          "number": 0,
          "size": 5
        },
        "_embedded" : {
          "values" : [
              {
                "label" : "Java",
                "count" : 100,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=Java"
                  }
                }
              },
              {
                "label" : "SQL",
                "count" : 80,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=Java"
                  }
                }
              },
              {
                "label" : "CSS",
                "count" : 10,
                "_links": {
                  "search" : {
                    "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&subject.equals=Java"
                  }
                }
              }
          ]
        }
      },
      {
        "name": "dateIssued",
        "facetType": "date",
        "facetLimit": 10,
        "minValue": "1940-03-15",
        "maxValue": "2017-11-06",
        "_links": {
          "self": {
            "href": "/api/discover/facets/dateIssued?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority"
          }
        },
        "page": {
          "number": 0,
          "size": 10
        },
        "_embedded": {
          "values": [
            {
              "label": "1940 - 1959",
              "count": 23370,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1940 TO 1959],equals"
                }
              }
            },
            {
              "label": "1960 - 1979",
              "count": 45044,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1960 TO 1979],equals"
                }
              }
            },
            {
              "label": "1980 - 1999",
              "count": 56128,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[1980 TO 1999],equals"
                }
              }
            },
            {
              "label": "2000 - 2017",
              "count": 51707,
              "type": "discover",
              "_links": {
                "search": {
                  "href": "http://dspace7-internal.atmire.com/rest/api/discover/search/objects?f.dateIssued=[2000 TO 2017],equals"
                }
              }
            }
          ]
        }
      }
    ]
  }
}
```

### Matching facet search results
**/api/discover/facets**

Provide access to the Discovery facet system (SOLR based). This endpoint returns the list of available facet fields that can be queried. The endpoint supports the following parameters:
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search will to be limited. If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml`[https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L28] that configuration will be returned. Otherwise the default configuration will be returned.
* `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored.

The list of returned facet fields will depend on the Discovery configuration: https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L86

The JSON response document is as follow
```json
{
  "id": null,
  "scope": null,
  "query": null,
  "appliedFilters": null,
  "sort": null,
  "configurationName": null,
  "type": "discover",
  "_links": {
    "first": {
      "href": "/api/discover/search/facets?page=0&size=10"
    },
    "self": {
      "href": "/api/discover/search/facets"
    },
    "next": {
      "href": "/api/discover/search/facets?page=1&size=10"
    },
    "last": {
      "href": "/api/discover/search/facets?page=9&size=10"
    }
  },
  "page" : {
    "size": 10,
    "number": 0
  },  
  "_embedded": {
    "facets": [
      {
        "name" : "author",
        "facetType": "text",
        "facetLimit": 10,
        "_links": {
          "self": {
            "href": "/api/discover/facets/author"
          }
        }   
      },
      {
        "name" : "subject",
        "facetType": "hierarchical",
        "facetLimit": 10,
        "_links": {
          "self": {
            "href": "/api/discover/facets/subject"
          }
        }
      },
      {
        "name" : "dateIssued",
        "facetType": "date",
        "facetLimit": 10,
        "hasMinMaxValues": true,
        "_links": {
          "self": {
            "href": "https://dspace7.4science.it/dspace-spring-rest/api/discover/facets/dateissued"
          }
        }
      },
      {
        "name" : "has_content_in_original_bundle",
        "facetType": "standard",
        "facetLimit": 2,
        "_links": {
          "self": {
            "href": "https://dspace7.4science.it/dspace-spring-rest/api/discover/facets/has_content_in_original_bundle"
          }
        }
      }
    ]
  }
}
```

### List values of a certain facet
**/api/discover/facets/<:facet-name>**

This endpoint returns a list of values that correspond to the given facet name. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records. Please note that this will be treat as a lucene/SOLR query so if special characters need to be literally searched escape them.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `f.<:filter-name>=<:filter-value>,<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `f.author=5df05073-3be7-410d-8166-e254369e4166,authority` or `f.title=rainbows,notcontains`.
* `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name be "count" (results ordered descending by the number of matching records) or "index" (results order alphabetically).

Example: TODO

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "appliedFilters": [
      {
        "filter" : "title",
        "operator" : "notcontains",
        "value" : "abcd",
        "label" : "abcd"
      },
      {
        "filter" : "author",
        "operator" : "authority",
        "value" : "1234",
        "label" : "Smith, Donald"
      }
  ],
  "sort" : {
    "by" : "index"
  },
  "page" : {
    	"size": 5,
    	"number": 0
  },
  "name" : "author",
  "facetType": "text",
  "facetLimit": 10,
  "_embedded" : {
    "values" : [
        {
          "label" : "Smith, Donald 2",
          "count" : 100,
          "_links": {
            "search" : {
              "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+2,equals"
            }
          }
        },
        {
          "label" : "Smith, Donald 1",
          "count" : 80,
          "_links": {
            "search" : {
              "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+1,equals"
            }
          }
        },
        {
          "label" : "Smith, Donald 3",
          "count" : 10,
          "_links": {
            "search" : {
              "href": "/api/discover/search/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&f.author=Smith,+Donald+3,equals"
            }
          }
        }
    ]
  },
  "_links": {
      "self": {
        "href": "/api/discover/facets/author?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=0&size=5"
      },
      "next": {
        "href": "/api/discover/facets/author?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&f.title=abcd,notcontains&f.author=1234,authority&page=1&size=5"
      }
  }
}
```

## Special configurations

### workspace
This configuration is used to retrieve all the submission done by the current user regardless to the status that they have reached (i.e. it returns workspaceitems, workflowtems and archived items).

Example

```
{
  "id" : null,
  "scope" : null,
  "query" : null,
  "appliedFilters" : null,
  "sort" : null,
  "configuration" : "workspace",
  "type" : "discover",
  "_links" : {
    "self" : {
      "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/search/objects?configuration=workspace"
    }
  },
  "_embedded" : {
    "searchResult" : {
      "_embedded" : {
        "objects" : [ {
          "hitHighlights" : null,
          "type" : "discover",
          "_links" : {
            "indexableObject" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8"
            }
          },
          "_embedded" : {
            "indexableObject" : {
              "id" : 8,
              "lastModified" : "2019-03-07T18:19:45.187+0000",
              "sections" : {
                "license" : {
                  "url" : null,
                  "acceptanceDate" : null,
                  "granted" : false
                },
                "upload" : {
                  "files" : [ ]
                },
                "collection" : "4631161d-9c72-4556-b554-15e0db82628d",
                "traditionalpagetwo" : { },
                "traditionalpageone" : {
                  "dc.title" : [ {
                    "value" : "Admin Workspace Item 1",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ],
                  "dc.date.issued" : [ {
                    "value" : "2010-07-23",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ]
                }
              },
              "type" : "workspaceitem",
              "_links" : {
                "collection" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8/collection"
                },
                "item" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8/item"
                },
                "submissionDefinition" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8/submissionDefinition"
                },
                "submitter" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8/submitter"
                },
                "self" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/8"
                }
              },
              "_embedded" : {
                "submitter" : {
					...
                },
                "item" : {
                  	...
                },
                "submissionDefinition" : {
                		...
                },
                "collection" : {
					...
	  		   }
  		     }
        }, {
          "hitHighlights" : null,
          "type" : "discover",
          "_links" : {
            "indexableObject" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9"
            }
          },
          "_embedded" : {
            "indexableObject" : {
              "id" : 9,
              "lastModified" : "2019-03-07T18:19:45.245+0000",
              "sections" : {
                "license" : {
                  "url" : null,
                  "acceptanceDate" : null,
                  "granted" : false
                },
                "upload" : {
                  "files" : [ ]
                },
                "collection" : "e1a05a4b-0a5f-4aec-9f18-a01b1080dab1",
                "traditionalpagetwo" : { },
                "traditionalpageone" : {
                  "dc.title" : [ {
                    "value" : "Admin Workspace Item 2",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ],
                  "dc.date.issued" : [ {
                    "value" : "2010-11-03",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ]
                }
              },
              "type" : "workspaceitem",
              "_links" : {
                "collection" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9/collection"
                },
                "item" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9/item"
                },
                "submissionDefinition" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9/submissionDefinition"
                },
                "submitter" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9/submitter"
                },
                "self" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems/9"
                }
              },
              "_embedded" : {
                "submitter" : {
					...
                },
                "item" : {
                  	...
                },
                "submissionDefinition" : {
                		...
                },
                "collection" : {
					...
	  		   }
  		     }
        }, {
          "hitHighlights" : null,
          "type" : "discover",
          "_links" : {
            "indexableObject" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3"
            }
          },
          "_embedded" : {
            "indexableObject" : {
              "id" : 3,
              "lastModified" : "2019-03-07T18:19:45.282+0000",
              "sections" : {
                "license" : {
                  "url" : null,
                  "acceptanceDate" : null,
                  "granted" : false
                },
                "upload" : {
                  "files" : [ ]
                },
                "collection" : "e1a05a4b-0a5f-4aec-9f18-a01b1080dab1",
                "traditionalpagetwo" : { },
                "traditionalpageone" : {
                  "dc.title" : [ {
                    "value" : "Admin Workflow Item 1",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ],
                  "dc.date.issued" : [ {
                    "value" : "2010-11-03",
                    "language" : "*",
                    "authority" : null,
                    "confidence" : -1
                  } ]
                }
              },
              "type" : "workflowitem",
              "_links" : {
                "collection" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3/collection"
                },
                "item" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3/item"
                },
                "submissionDefinition" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3/submissionDefinition"
                },
                "submitter" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3/submitter"
                },
                "self" : {
                  "href" : "https://dspace7.4science.it/dspace-spring-rest/api/workflow/workflowitems/3"
                }
              },
			"_embedded" : {
                "submitter" : {
					...
                },
                "item" : {
                  	...
                },
                "submissionDefinition" : {
                		...
                },
                "collection" : {
					...
	  		   }
  		     }
           }
        } ]
      },
      "_links" : {
        "self" : {
          "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/search/objects?configuration=workspace"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 20,
        "totalPages" : 1,
        "totalElements" : 3
      }
    },
    "facets" : [ {
      "name" : "namedresourcetype",
      "facetType" : "text",
      "facetLimit" : 10,
      "_links" : {
        "self" : {
          "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/facets/namedresourcetype?configuration=workspace"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 10
      },
      "_embedded" : {
        "values" : [ {
          "label" : "Workspace",
          "count" : 2,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/search/objects?configuration=workspace&f.namedresourcetype=workspace,authority"
            }
          }
        }, {
          "label" : "Workflow",
          "count" : 1,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/search/objects?configuration=workspace&f.namedresourcetype=workflow,authority"
            }
          }
        } ]
      }
    }, {
      "name" : "itemtype",
      "facetType" : "text",
      "facetLimit" : 10,
      "_links" : {
        "self" : {
          "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/facets/itemtype?configuration=workspace"
        }
      },
      "_embedded" : {
        "values" : [ ]
      }
    }, {
      "name" : "dateIssued",
      "facetType" : "date",
      "facetLimit" : 5,
      "minValue" : "1990-02-13",
      "maxValue" : "2010-11-03",
      "_links" : {
        "self" : {
          "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/facets/dateIssued?configuration=workspace"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 5
      },
      "_embedded" : {
        "values" : [ {
          "label" : "2010",
          "count" : 3,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "https://dspace7.4science.it/dspace-spring-rest/api/discover/search/objects?configuration=workspace&f.dateIssued=2010,equals"
            }
          }
        } ]
      }
    } ]
  }
}
```

### workflow
This configuration is used to retrieve all the tasks relevant for the current user. This mean pool tasks that can be claimed or already claimed tasks.
Example
```
{
  "id" : null,
  "scope" : null,
  "query" : null,
  "appliedFilters" : null,
  "sort" : null,
  "configuration" : "workflow",
  "type" : "discover",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/discover/search/objects?configuration=workflow"
    }
  },
  "_embedded" : {
    "searchResult" : {
      "_embedded" : {
        "objects" : [ {
          "hitHighlights" : null,
          "type" : "discover",
          "_links" : {
            "indexableObject" : {
              "href" : "http://localhost/api/workflow/pooltasks/1"
            }
          },
          "_embedded" : {
            "indexableObject" : {
              "id" : 1,
              "step" : "reviewstep",
              "action" : "claimaction",
              "type" : "pooltask",
              "_links" : {
                "eperson" : {
                  "href" : "http://localhost/api/workflow/pooltasks/1/eperson"
                },
                "group" : {
                  "href" : "http://localhost/api/workflow/pooltasks/1/group"
                },
                "workflowitem" : {
                  "href" : "http://localhost/api/workflow/pooltasks/1/workflowitem"
                },
                "self" : {
                  "href" : "http://localhost/api/workflow/pooltasks/1"
                }
              },
              "_embedded" : {
                "eperson" : null,
                "group" : {
                	...
                },
                "workflowitem" : {
                  	...
                }
              }
            }
          }
        }, {
          "hitHighlights" : null,
          "type" : "discover",
          "_links" : {
            "indexableObject" : {
              "href" : "http://localhost/api/workflow/pooltasks/3"
            }
          },
          "_embedded" : {
            "indexableObject" : {
              "id" : 3,
              "step" : "reviewstep",
              "action" : "claimaction",
              "type" : "pooltask",
              "_links" : {
                "eperson" : {
                  "href" : "http://localhost/api/workflow/pooltasks/3/eperson"
                },
                "group" : {
                  "href" : "http://localhost/api/workflow/pooltasks/3/group"
                },
                "workflowitem" : {
                  "href" : "http://localhost/api/workflow/pooltasks/3/workflowitem"
                },
                "self" : {
                  "href" : "http://localhost/api/workflow/pooltasks/3"
                }
              },
              "_embedded" : {
                "eperson" : null,
                "group" : {
                 	...
                },
                "workflowitem" : {
					...
                }
              }
            }
          }
        } ]
      },
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/discover/search/objects?configuration=workflow"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 20,
        "totalPages" : 1,
        "totalElements" : 2
      }
    },
    "facets" : [ {
      "name" : "namedresourcetype",
      "facetType" : "text",
      "facetLimit" : 10,
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/discover/facets/namedresourcetype?configuration=workflow"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 10
      },
      "_embedded" : {
        "values" : [ {
          "label" : "Waiting for Controller",
          "count" : 2,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "http://localhost/api/discover/search/objects?configuration=workflow&f.namedresourcetype=waitingforcontroller,authority"
            }
          }
        } ]
      }
    }, {
      "name" : "itemtype",
      "facetType" : "text",
      "facetLimit" : 10,
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/discover/facets/itemtype?configuration=workflow"
        }
      },
      "_embedded" : {
        "values" : [ ]
      }
    }, {
      "name" : "dateIssued",
      "facetType" : "date",
      "facetLimit" : 5,
      "minValue" : "1990-02-13",
      "maxValue" : "2010-11-03",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/discover/facets/dateIssued?configuration=workflow"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 5
      },
      "_embedded" : {
        "values" : [ {
          "label" : "2010",
          "count" : 2,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "http://localhost/api/discover/search/objects?configuration=workflow&f.dateIssued=2010,equals"
            }
          }
        } ]
      }
    }, {
      "name" : "submitter",
      "facetType" : "text",
      "facetLimit" : 10,
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/discover/facets/submitter?configuration=workflow"
        }
      },
      "page" : {
        "number" : 0,
        "size" : 10
      },
      "_embedded" : {
        "values" : [ {
          "label" : "first (admin) last (admin)",
          "count" : 1,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "http://localhost/api/discover/search/objects?configuration=workflow&f.submitter=9ad6a949-9f4f-4504-8a5a-881274ac0bc3,authority"
            }
          }
        }, {
          "label" : "first last",
          "count" : 1,
          "type" : "discover",
          "_links" : {
            "search" : {
              "href" : "http://localhost/api/discover/search/objects?configuration=workflow&f.submitter=014186a8-aae1-45e9-879e-48416acd6248,authority"
            }
          }
        } ]
      }
    } ]
  }
}
```
