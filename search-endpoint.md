# Discovery Search Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Search Endpoint
**/api/discover/search**   

This endpoint provides the functionality of the Discovery search screen: http://demo.dspace.org/xmlui/discover.

Provide access to the Discovery search system (SOLR based). This endpoint returns the list of available DSpace object (DSO) types that can be searched:
* all: Execute a query over all available DSO types.
* items: Only search the DSpace items.
* communities: Search within the DSpace community records.
* collections: Limit the search to DSpace collection records.

Example: TODO

### Search with a given DSO type
**/api/discover/search/<:dso-type>**

This endpoint will provide detailed information on the Discovery search configuration that can be used to build complex searches.

It supports the following parameters:
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search will to be limited. If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml`[https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L28] that configuration will be returned. Otherwise the default configuration will be returned.

The JSON response document is as follow
```json
{
  "filters": [
    {
      "filter" : "title",
    },
    {
      "filter" : "author",
    },
    {
      "filter" : "type",
    }
  ],
  "operators": [
    {
      "operator" : "contains",
    },
    {
      "operator" : "notcontains",
    },
    {
      "operator" : "authority",
    }
  ],
  "sortOptions": [
    {
      "name": "title",
      "metadata": "dc.title"
    },
    {
      "name": "dateissued",
      "metadata": "dc.date.issued"
    },
    {
      "name": "dateaccessioned",
      "metadata": "dc.date.accessioned"
    }
  ],
}
```

* filters: Provides a list of advanced search filters that can be used to limit the result set as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L97
* operators: A list of supported operators that can be combined on each search filter.
* sortOptions: The sort options available for this query type as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L112

Exposed links:
* objects: link to get the actual list of objects that match the search query
* facets: link to get the list of facet values and counts associated with this search query as configured in https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L86

### Matching DSpace objects search results
**/api/discover/search/<:dso-type>/objects**

This endpoint returns a list of DSpace Objects that match the given type. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `<:filter-name>.<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `author.authority=5df05073-3be7-410d-8166-e254369e4166` or `title.notcontains=rainbows`.
* `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name must match a value returned by the parent search endpoint (see above) or *default*, followed by a comma and the order direction. For example `sort=default,asc` or `sort=dateissued,desc`.

Example: TODO

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "filters": [
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
    "by" : "dateissued",
    "order" : "asc"
  },
  "page": {
    	"size": 5,
    	"totalElements": 14,
    	"totalPages": 3,
    	"number": 0
  },
  "results" : [
    {
      "dspaceObject" : {
        ... --> Partial object
      },
      "hitHighlights": {
        "dc.description.abstract" : "This is the <em>very cool</em> abstract of this item",
        "dc.publisher" : "My <em>very cool</em> publisher",
      }
    },
    {
      "dspaceObject" : {
        ... --> Partial object
      },
      "hitHighlights": { }
    }
  ],
  "_links": {
      "first": {
        "href": "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&page=0&size=5"
      },
      "self": {
        "href": "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&page=0&size=5"
      },
      "next": {
        "href": "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&page=1&size=5"
      },
      "last": {
        "href": "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&page=2&size=5"
      }
  }
}
```

### Matching facet search results
**/api/discover/search/<:dso-type>/facets**

This endpoint returns a list of facet values and counts based on the current query. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `<:filter-name>.<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `author.authority=5df05073-3be7-410d-8166-e254369e4166` or `title.notcontains=rainbows`.
* There is no paging here since the number of facet values displayed in the Discovery search screen is configured in the backend.

Example: TODO

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "filters": [
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
  "sort" :
  {
    "by" : "dc.title",
    "order" : "asc"
  },
  "facets" : [
    {
      "name" : "author",
      "results" : [
          {
            "value" : "Smith, Donald 2",
            "count" : 100,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+2"
            }
          },
          {
            "value" : "Smith, Donald 1",
            "count" : 80,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+1"
            }
          },
          {
            "value" : "Smith, Donald 3",
            "count" : 10,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+3"
            }
          }
      ]
    },
    {
      "name" : "subject",
      "results" : [
          {
            "value" : "Java",
            "count" : 100,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&subject.equals=Java"
            }
          },
          {
            "value" : "SQL",
            "count" : 80,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&subject.equals=Java"
            }
          },
          {
            "value" : "CSS",
            "count" : 10,
            "_links": {
              "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&subject.equals=Java"
            }
          }
      ]
    }
  ],
  "_links": {
      "self": {
        "href": "/api/discover/search/<:dso-type>/facets?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234"
      }
  }
}
```

## Facet Endpoint
**/api/discover/facets**   

This endpoint provides the functionality of the Discovery search filter screens, e.g. http://demo.dspace.org/xmlui/search-filter?field=author&order=COUNT

Provide access to the Discovery facet system (SOLR based). This endpoint returns the list of available facet fields that can be queried. The endpoint supports the following parameters:
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search will to be limited. If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml`[https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L28] that configuration will be returned. Otherwise the default configuration will be returned.

The list of returned facet fields will depend on the Discovery configuration: https://github.com/DSpace/DSpace/blob/master/dspace/config/spring/api/discovery.xml#L86

The JSON response document is as follow
```json
{
  "facets": [
    {
      "name" : "author",
      "type" : "string"
    },
    {
      "name" : "dateissued",
      "type" : "date"
    },
    {
      "name" : "subject",
      "type" : "string"
    }
  ]
}
```

### List values of a certain facet
**/api/discover/facets/<:facet-name>**

This endpoint returns a list of values that correspond to the given facet name. The result can be refined using the following parameters:
* `query`: The discovery search string to will be used to match records.
* `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
* `<:filter-name>.<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by parent search endpoint (see above). For example `author.authority=5df05073-3be7-410d-8166-e254369e4166` or `title.notcontains=rainbows`.
* `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name be "count" (results ordered descending by the number of matching records) or "index" (results order alphabetically).

Example: TODO

The returned JSON response will be like:

```json
{
  "query":"my query",
  "scope":"9076bd16-e69a-48d6-9e41-0238cb40d863",
  "filters": [
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
    "by" : "dateissued",
    "order" : "asc"
  },
  "page" : {
    	"size": 5,
    	"totalElements": 14,
    	"totalPages": 3,
    	"number": 0
  },
  "results" : [
      {
        "value" : "Smith, Donald 2",
        "count" : 100,
        "_links": {
          "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+2"
        }
      },
      {
        "value" : "Smith, Donald 1",
        "count" : 80,
        "_links": {
          "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+1"
        }
      },
      {
        "value" : "Smith, Donald 3",
        "count" : 10,
        "_links": {
          "self" : "/api/discover/search/<:dso-type>/objects?query=my+query&scope=9076bd16-e69a-48d6-9e41-0238cb40d863&title.notcontains=abcd&author.authority=1234&author.equals=Smith,+Donald+3"
        }
      }
  ]
}
```
