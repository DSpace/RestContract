# Endpoints
[REST Overview Documentation](README.md)

## Available Endpoints
* /api/core/sites
* [/api/core/communities](communities.md)
* /api/core/collections
* [/api/core/items](items.md)
* [/api/core/itemtemplates](itemtemplates.md)
* [/api/core/bitstreams](bitstreams.md)
* [/api/core/bitstreamformats](bitstreamformats.md)
* [/api/core/bundles](bundles.md)
* /api/core/sites
* [/api/core/metadatafields](metadatafields.md)
* [/api/core/metadataschemas](metadataschemas.md)
* [/api/core/epersons](epersons.md)
* /api/core/groups
* [/api/core/{model}/search](search-rels.md)
* [/api/authn/login](authentication.md#Login)
* [/api/authn/logout](authentication.md#Logout)
* [/api/authn/status](authentication.md#Status)
* [/api/config/harvestermetadata](harvestermetadata.md)
* [/api/config/submissiondefinitions](submissiondefinitions.md)
* [/api/config/submissionsections](submissionsections.md)
* [/api/config/submissionforms](submissionforms.md)
* [/api/config/submissionuploads](submissionuploads.md)
* [/api/config/workflowdefinitions](workflowdefinitions.md)
* [/api/config/workflowsteps](workflowsteps.md)
* [/api/discover/browses](browses.md)
* [/api/discover/search](search-endpoint.md)
* [/api/submission/workspaceitems](workspaceitems.md)
* [/api/integration/authorities](authorities.md)
* [/api/workflow/workflowitems](workflowitems.md)
* [/api/workflow/pooltasks](pooltasks.md)
* [/api/workflow/claimedtasks](claimedtasks.md)

## Endpoints Under Development/Discussion
* [/api/authz/resourcepolicies](resourcepolicies.md)
* [/api/authz/authorizations](authorizations.md)
* [/api/authz/features](features.md)
* [/api/statistics](statistics.md)
* [/api/tools/itemrequests](item-requests.md)

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
