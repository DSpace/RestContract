# Facets Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main endpoint

**/api/discover/facets**

This endpoint returns the list of available facet fields that can be queried. The endpoint supports the following parameters:

- `scope`: UUID of a specific DSpace container (site, community or collection). If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml` that configuration will be used. Otherwise the default configuration will be used. The sidebar facets of the found configuration are returned.
- `configuration`: The name of a Discovery configuration of which the sidebar facets should be returned. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored.

## Single Facet

**/api/discover/facets/<:name>**

This endpoint returns a single facet. The endpoints supports the following parameters:

- `scope`: UUID of a specific DSpace container (site, community or collection). If the scope has a specific DSpace configuration defined in `config/spring/api/discovery.xml` that configuration will be used. Otherwise the default configuration will be used. The requested sidebar facet should be in this configuration.
- `configuration`: The name of a Discovery configuration of which one of the sidebar facets should be returned. If the provided scope already has a specific Discovery configuration defined, than this parameter will be ignored.

Sample JSON:

```
{
  "name": "author",
  "id": "author",
  "facetType": "text",
  "facetLimit": 5,
  "minValue": "Doe, Jane",
  "maxValue": "Smith, John",
  "openByDefault": true,
  "type": "facet"
}
```

Exposed links: 

- `values`: link to the list of values that correspond to the facet

## Linked entities 

### Values

**GET /api/discover/facets<:name>/values**

It returns the facet values corresponding to the given facet. The endpoint supports the following parameters:

- `query`: The discovery search string to will be used to match records. Please note that this will be treated as a Solr/lucene query, which means Solr query syntax is available. However, any special characters must be URL or percent encoded (i.e. %xx).
- `dsoType`: Limit the search to a specific DSpace Object type:
    - all: Execute a query over all available DSO types.
    - item: Search within DSpace items.
    - community: Search within DSpace communities.
    - collection: Search within DSpace collections.
- `scope`: UUID of a specific DSpace container (site, community or collection) to which the search has to be limited, e.g. `scope=9076bd16-e69a-48d6-9e41-0238cb40d863`.
- `configuration`: The name of a Discovery configuration that should be used by this search. If the provided scope already has a specific Discovery configuration defined, then this parameter will be ignored.
- `f.<:filter-name>=<:filter-value>,<:filter-operator>`: Advanced search filter that has to be used to filter the result set. The `filter-name` and `filter-operator` must match a value returned by the DiscoveryConfigurations endpoint (see [here](discoveryconfigurations.md)). For example `f.author=5df05073-3be7-410d-8166-e254369e4166,authority` or `f.title=rainbows,notcontains`. If the filter operator is absent or invalid a "422 Unprocessable Entity" will be returned.
- `page`, `size` & `sort` [see pagination](README.md#Pagination): the sort name must match a value returned by the DiscoveryConfigurations endpoint (see [here](discoveryconfigurations.md)). or *default*, followed by a comma and the order direction. For example `sort=default,asc` or `sort=dateissued,desc`.

```
{
  "self": {},
  "_embedded": {
    "values": [
      {
        "label": "Smith, John",
        "count": 2,
        "authorityKey": null,
        "type": "discover",
        "_links": {
          "search": {
            "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?f.author=Smith,%20John,equals"
          }
        }
      },
      {
        "label": "Doe, Jane",
        "count": 1,
        "authorityKey": null,
        "type": "discover",
        "_links": {
          "search": {
            "href": "http://localhost:8080/server/api/discover/searchresults/search/objects?f.author=Doe,%20Jane,equals"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/discover/facets/author/values"
    }
  },
  "page": {
    "number": 0,
    "size": 20,
    "totalPages": 1,
    "totalElements": 2
  }
}
```
