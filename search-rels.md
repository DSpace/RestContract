# Endpoint Searches and Endpoint Relationships
[REST Overview Documentation](README.md)

Resources collection should expose search methods to return specific subsets depending on the user requirements. Such as the items from a specific submitter, the top level communities, etc.
All these methods should be exposed under <resources-collection-endpoint>/search/<method-name> at <resources-collection-endpoint>/search a HAL document should list all the available search methods for the resources collection

**Please note that the Discovery search is a completely separate topic**

## Available Search
not yet

## Under Development
* /communities/search/top - list top level communities (shoud this be /core/search/top-communities)

## Available Relationships
* /communities/:uuid/collections
* /communities/:uuid/logo
* /collections/:uuid/logo

## Under Development
* /community/(id)/subCommunities

## Proposed Ideas
* /communities/:uuid/parentCommunity
* /collections/:uuid/parentCommunity
* /collections/:uuid/workflow
* /collections/:uuid/templateitem

## Rejected Ideas
