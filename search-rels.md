# Search methods on collection endpoints
[REST Overview Documentation](README.md)

Individual collection endpoints (e.g. `/api/core/{model}`) may expose search methods (`/api/core/{model}/search`) to return specific subsets of resources. This allows for filtering of the collection (group of resources) to only return specific resources that match the search requirements. Some examples include a search endpoint for only returning top-level Communities (e.g. `/api/core/communities/search/top`) or a search endpoint for only returning WorkspaceItems from a specific Submitter (e.g. `/api/submission/workspaceitems/search/findBySubmitter?uuid=<:submitter-uuid>`).

Search endpoints on a collection of resources should act as follows:
* All available search methods (for the given resource) should be exposed under `/api/core/{model}/search`. The result should be a HAL document.
* Individual search methods should be exposed under `/api/core/{model}/search/{method-name}`

**Please note that the [Discovery search](search-endpoint.md) is a completely separate topic**

## Endpoints that have Search methods
* [/api/core/communities](communities.md)
* [/api/core/metadatafields/](metadatafields.md)
* [/api/config/submissiondefinitions](submissiondefinitions.md)
* [/api/submission/workspaceitems](workspaceitems.md)
* [/api/workflow/claimedtasks](claimedtasks.md)
* [/api/workflow/polltasks](polltasks.md)
* [/api/workflow/workflowitems](workflowitems.md)
