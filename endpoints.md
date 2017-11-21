# Endpoints
[REST Overview Documentation](README.md)

## Available Endpoints
* /api/core/sites
* [/api/core/communities](communities.md)
* /api/core/collections
* [/api/core/items](items.md)
* [/api/core/bitstreams](bitstreams.md)
* /api/core/bitstreamformats
* /api/core/sites
* /api/core/metadatafields
* /api/core/metadataschemas
* /api/core/epersons
* /api/core/groups
* [/api/core/{model}/search](search-rels.md)
* [/api/config/submissiondefinitions](submissiondefinitions.md)
* [/api/config/submissionsections](submissionsections.md)
* [/api/config/submissionforms](submissionforms.md)
* [/api/discover/browses](browses.md)
* [/api/integration/authorities](authorities.md)

## Endpoints Under Development/Discussion
* [/api/discover/search](search-endpoint.md)
* [/api/config/submissionuploads](submissionforms.md)
* [/api/integration/authorities](authorities.md)
* [/api/submission/workspaceitems](workspaceitems.md)
* /api/authn/login
* /api/authn/logout
* /api/authn/status
* /api/authz/resourcepolicies
 
## Other Endpoints (raw list)
* /api/authorize/(dso)
* /api/curate
* /api/export
* /api/app/bulkmetadataedit
* /api/statistics/(dso)
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

