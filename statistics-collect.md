# Collecting user interactions
[Back to the list of all defined endpoints](endpoints.md)

## Collect Endpoint
**POST /api/statistics/collect**

To track a user interaction, make an HTTP POST request to this endpoint.

The sections below describe the parameters for a user interaction request. All other information about the interaction can be derived from the request itself.

### Common Parameters
- `type`: The type of user interaction to track. Possible values:
  - `view`: a page view
  - `search`: a search operation.

### Parameters for page views
- `targetId`: The id of the object the user viewed
- `targetType`: The type of the object the user viewed

### Parameters for searches
- `query`: The discovery search string.
- `dsoType`: Limits the search to a specific DSpace Object type. Possible values:
     - `all`
     - `item`
     - `community`
     - `collection`
- `scope`: The UUID of a specific DSpace container (site, community or collection) to which the search has to be limited.
- `configuration`: The name of a Discovery configuration that was used by the search.
- `appliedFilters`: An array of search filters  used to filter the result set. For more information take a look at the [search documentation](search-endpoint.md#matching-dspace-objects-search-results).
- `page`: An object that describes the pagination status. For more information take a look at the [pagination documentation](README.md#Pagination).
- `sort`: An object that describes the sort status. For more information take a look at the [pagination documentation](README.md#Pagination).


### Status codes:

- `201` Created: if the operation succeeded
- `400` Bad request: if any of the parameters are missing or invalid.

### Example item page view:

```json
{
  "type": "view",
  "targetId": "43f9bb3e-f90d-458f-9858-7e4589481d18",
  "targetType": "item",
}
```

### Example search:

```json
{
  "type": "search",
  "query": "Lorem ipsum",
  "scope": "a3e80c6d-a83f-4c35-9f27-8718598c6c17",
  "configuration": "default",
  "appliedFilters": [
      {
        "filter" : "title",
        "operator" : "notcontains",
        "value" : "dolor sit",
        "label" : "dolor sit"
      },
      {
        "filter" : "author",
        "operator" : "authority",
        "value" : "9zvxzdm4qru17or5a83wfgac",
        "label" : "Amet, Consectetur"
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
  }
}
```
