# DiscoveryConfigurations Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main endpoint

**/api/discover/discoveryconfigurations**

This endpoint is unsupported.

## Single DiscoveryConfiguration

**/api/discover/discoveryconfigurations/<:name>**

This endpoint returns the requested DiscoveryConfiguration. If there's no DiscoveryConfiguration with the provided name, the `default` configuration is returned.

If the name equals `scope`, the endpoint expects a `uuid` parameter. The UUID is resolved to a DSO. If that DSO has a specific configuration defined in `config/spring/api/discovery.xml`, that configuration will be returned. If the UUID can't be resolved or the DSO has no specific configuration defined, the `default` configuration is returned.

Sample JSON:

```
{
  "id": "workspace",
  "type": "discoveryconfiguration"
}
```

Exposed links:

- `searchfilters`: link to the search filters defined on this configuration
- `sortoptions`: link to the sort options defined on this configuration
- `defaultsortoption`: link to the default sort option defined on this configuration

## Linked entities
 
### Search filters

**GET /api/discover/discoveryconfigurations/<:name>/searchfilters**

It returns the search filters defined on the configuration.

Sample JSON:

```
{
  "self": {},
  "_embedded": {
    "searchfilters": [
      {
        "filter": "namedresourcetype",
        "hasFacets": true,
        "filterType": "authority",
        "pageSize": 0,
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
          },
          {
            "operator": "query"
          }
        ],
        "openByDefault": false,
        "type": "searchfilter"
      },
      {
        "filter": "itemtype",
        "hasFacets": true,
        "filterType": "text",
        "pageSize": 0,
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
          },
          {
            "operator": "query"
          }
        ],
        "openByDefault": false,
        "type": "searchfilter"
      },
      {
        "filter": "dateIssued",
        "hasFacets": true,
        "filterType": "date",
        "pageSize": 10,
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
          },
          {
            "operator": "query"
          }
        ],
        "openByDefault": false,
        "type": "searchfilter"
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
      "href": "http://localhost:8080/server/api/discover/discoveryconfigurations/workspace/searchfilters"
    }
  }
}
```

### Sort options

**GET /api/discover/discoveryconfigurations/<:name>/sortoptions**

It returns the sort options defined on this configuration.

Sample JSON:

```
{
  "self": {},
  "_embedded": {
    "sortoptions": [
      {
        "name": "lastModified",
        "sortOrder": "desc",
        "type": "sortoption"
      },
      {
        "name": null,
        "sortOrder": "desc",
        "type": "sortoption"
      },
      {
        "name": "dc.title",
        "sortOrder": "asc",
        "type": "sortoption"
      },
      {
        "name": "dc.date.issued",
        "sortOrder": "desc",
        "type": "sortoption"
      }
    ]
  },
  "page": {
    "number": 0,
    "size": 20,
    "totalPages": 1,
    "totalElements": 4
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/discover/discoveryconfigurations/workspace/sortoptions"
    }
  }
}
```

### Default sort option

**GET /api/discover/discoveryconfigurations/<:name>/defaultsortoption**

It returns the default sort option defined on this configuration.

Sample JSON:

```
{
  "name": "lastModified",
  "sortOrder": "desc",
  "type": "sortoption",
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/discover/discoveryconfigurations/workspace/defaultsortoption"
    }
  }
}
```
