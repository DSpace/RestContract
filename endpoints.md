# Endpoints
[REST Overview Documentation](README.md)

## Available Endpoints
* [/api/core/communities](communities.md)
* [/api/core/collections](collections.md)
* [/api/core/items](items.md)
* [/api/core/itemtemplates](itemtemplates.md)
* [/api/core/bitstreams](bitstreams.md)
* [/api/core/bitstreamformats](bitstreamformats.md)
* [/api/core/bundles](bundles.md)
* [/api/core/metadatafields](metadatafields.md)
* [/api/core/metadataschemas](metadataschemas.md)
* [/api/core/epersons](epersons.md)
* [/api/eperson/orcidqueues](orcidqueues.md)
* [/api/eperson/orcidhistories](orcidhistories.md)
* [/api/eperson/profiles](profiles.md)
* [/api/core/groups](epersongroups.md)
* [/api/core/{model}/search](search-rels.md)
* [/api/authn/login](authentication.md#Login)
* [/api/authn/logout](authentication.md#Logout)
* [/api/authn/status](authentication.md#Status)
* [/api/config/harvestermetadata](harvestermetadata.md)
* [/api/config/submissiondefinitions](submissiondefinitions.md)
* [/api/config/submissionsections](submissionsections.md)
* [/api/config/submissionforms](submissionforms.md)
* [/api/config/submissionuploads](submissionuploads.md)
* [/api/config/submissionaccessoptions](submissionaccessoptions.md)
* [/api/config/workflowdefinitions](workflowdefinitions.md)
* [/api/config/workflowsteps](workflowsteps.md)
* [/api/discover/browses](browses.md)
* [/api/discover/search](search-endpoint.md)
* [/api/pid](identifiers.md)
* [/api/submission/workspaceitems](workspaceitems.md)
* [/api/submission/vocabularies](vocabularies.md)
* [/api/submission/vocabularyEntryDetails](vocabularyEntryDetails.md)
* [/api/system/systemwidealerts](systemwidealerts.md)
* [/api/versioning/version](version.md)
* [/api/versioning/versionhistory](versionhistory.md)
* [/api/workflow/workflowitems](workflowitems.md)
* [/api/workflow/pooltasks](pooltasks.md)
* [/api/workflow/claimedtasks](claimedtasks.md)
* [/api/tools/feedbacks](feedbacks.md)
* [/api/authz/resourcepolicies](resourcepolicies.md)
* [/api/config/bulkaccessconditionoptions](bulkaccessconditionoptions.md)
* [/api/authz/authorizations](authorizations.md)
* [/api/authz/features](features.md)
* [/api/statistics](statistics.md)
* [/api/tools/itemrequests](item-requests.md)
* [/api/contentreport/filteredcollections](contentreport-filteredcollections.md)
* [/api/contentreport/filtereditems](contentreport-filtereditems.md)

## Actuator endpoints
The following endpoints are implemented using [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.enabling) and are enabled by default:
* /actuator/info 
* /actuator/health

For more details see the [/actuator endpoint](actuator.md)

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

## Other Endpoints not under /api context

* /oai
  * endpoint for OAI-PMH data provider (if enabled)
* /opensearch
  * endpoint for opensearch service
* [/signposting](signposting.md)
  * endpoint returning information for linkset (signposting)
* /sword
  * endpoint for sword (if enabled)
* /sword2
  * endpoint for sword2 (if enabled)

