# Endpoint' Search methods
[REST Overview Documentation](README.md)

Resources collection should expose search methods to return specific subsets depending on the user requirements. Such as the items from a specific submitter, the top level communities, etc.
All these methods should be exposed under <resources-collection-endpoint>/search/<method-name> at <resources-collection-endpoint>/search a HAL document should list all the available search methods for the resources collection

**Please note that the [Discovery search](search-endpoint.md) is a completely separate topic**

## Endpoint that have Search methods
* /core/communities
* /config/submissiondefinitions

## Under Development
* /submission/workspaceitems