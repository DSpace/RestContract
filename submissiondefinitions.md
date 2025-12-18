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
* sections: list of submission-section included in this definition

### Search methods
#### findByCollection
**/api/config/submissiondefinitions/search/findByCollection?uuid=<:collection-uuid>**

It returns the submission definition that apply to a specific collection eventually fallback to the default configuration 

### Linked entities
#### sections
**/api/config/submissiondefinitions/<:definition-name>/sections**

It returns the list of submission-sections used in the submission-definition, see [submissionsections.md](submissionsections.md#Single Submission-Section)
