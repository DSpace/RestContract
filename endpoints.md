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
* /api/discover/(dso)?(sort)
  * "context" contains the following
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
* /api/user/eperson
* /api/user/epersongroup
* /api/authorize/(dso)
* /api/submit
* /api/workflow
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

