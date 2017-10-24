# Submission-Definitions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A submission-definition is defined globally or per collection. It defines an overall submission process, consisting of a series of "sections" (previously than DSpace 7 known as Steps).

## Main Endpoint
**/api/config/submissiondefinitions**   

Provide access to the configured submission definitions. It returns the list of existent submission-definitions.

Example: to be provided

## Single Submission-Definition
**/api/config/submissiondefinitions/<:definition-name>**

Provide detailed information about a specific submission-definition. The JSON response document is as follow
```json
{
  "name": "traditional",
  "isDefault": true,
  "type": "submission-definition"
}

```

Exposed links:
* collections: list of collections that explicitly use such submission-definition
* sections: list of submission-section included in this definition

### Search methods
#### findByCollection
**/api/config/submissiondefinitions/search/findByCollection?uuid=<:collection-uuid>**

It returns the submission definition that apply to a specific collection eventually fallback to the default configuration 

### Linked entities
#### collections
**/api/config/submissiondefinitions/<:definition-name>/collections**

It returns the list of collection that make an explicit use of the submission-definition. If a collection doesn't specify the submission-definition to be used the default mapping apply but this collection is not included in the list returned by this method

#### sections
**/api/config/submissiondefinitions/<:definition-name>/sections**

It returns the list of submission-sections used in the submission-definition, see [submissionsections.md](submissionsections.md#Single Submission-Section)
