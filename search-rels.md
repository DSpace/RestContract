# Endpoint Searches and Endpoint Relationships
[REST Overview Documentation](README.md)

## Available Search
* /communities/search/top - list top level communities (shoud this be /core/search/top-communities)

## Search Functions that Need to be Implemented
* /core/search/(dso/id)?query=(query)&(facetname)=(val)&sort=(sort)
  * if dso/id is omitted, search is site wide
  * if dso/id is provided, search is constrained to a specific community or collection
  * query - full text search to run, optional
  * facetname - dso specific facet search (one or more may be provided)
  * sort - sort to apply to results, available sort values may be global or may be dso-specific

## Available Relationships
* /community/(id)/parentCommunity
* /collection/(id)/parentCommunity

## Under Development
* /community/(id)/subCommunities
* /community/(id)/collection

## Proposed Ideas
* /(dso)/search?(query)&(facet=value)&(sort=sort)
* /collection/(id)/workflow
* /collection/(id)/templateitem

## Rejected Ideas
