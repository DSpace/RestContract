# Endpoints
[REST Overview Documentation](README.md)

## Available Endpoints
* /api/core/communities
* /api/core/collections
* /api/core/sites
* /api/core/bitstreams
* /api/core/bitstreamformats

## Endpoints Under Development
* /api/core/sites

## Proposed Endpoints
* /api/discover/search
  * to provide access the the discovery search. The supported parameters could be:
    * profile (similar to the current location parameter but more general so that we can provide different configuration for the repository, for a community, etc.)
    * facet (values to apply, to exclude, etc.)
    * query (the generic query string to apply)
    * sort (the sort conditions)
  * Returns "context" which contains the following
    * Item counts
    * All items accessible to user by context default sort
    * All items accessible to user by selected sort
    * All facets counts for context
    * All filters available for the context
  * Get context for the full site
    * Get all items for a specific community or collection
    * Get all items for a full text search
    * Get all items for a faceted search
    * Get all items for a complex search
* /api/discover/browse
  * to provide access to the browse system. it should return the list of available browse indexes. 
  * On a specific browse indexes /api/discover/browse/ the supported parameters could be:
    * sort
    * filter value (on metadata browse)
* /api/user/eperson
* /api/user/epersongroup
* /api/authorize/(dso)
* /api/curate
* /api/export
* /api/app/bulkmetadataedit
* /api/statistics/(dso)
* /api/authority
* /api/configuration
  * return relevant configuration values to the client
    * DSpace version
    * Root url
    * Site name
    * Contact info
    * Root handle
    * Supported themes
    * Default language
    * Languages supported
  * question, how will we mark / designate the values that are safe to expose?
* /api/feature
  * return the status of features that are enabled for a site
    * statistics viewing
  * return the status of features that are enabled for authorized users
    * item versioning

