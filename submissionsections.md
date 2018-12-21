# Submission-Sections Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A submission-section represents a single section in the submission process. Depending on its type, it may include a configurable input form (if type='submission-form'). However, not all sections are currently configurable, and other types of steps include file upload, embargo/access rights, creative commons licensing, etc.
A list of the [available sectionType is provided here](submissionsection-types.md)

## Main Endpoint
**/api/config/submissionsections**   

Provide access to the configured submission sections. It returns the list of existent submission-sections.

Example: to be provided

## Single Submission-Definition
**/api/config/submissionsections/<:section-name>**

Provide detailed information about a specific submission-section. The JSON response document is as follow
```json
{
  	id: "id-of-the-submission-form-page",
  	header: "First page",
  	mandatory: true,
  	sectionType: "submission-form",
  	scope: null,
  	type: "submissionsection",
  	_links: {
  		"config" : "<dspace-url>/config/submissionforms/<:id-of-the-submission-form-page>" 
  	}
}
```

* the *header* attribute is the label or the i18n key to use to present the section to the user
* the *mandatory* attribute defines if the section MUST be used by each submission. Otherwise, the user is allowed to enable/disable the section interacting with the workspaceitem
* the *scope* attribute can be null or one of the values: workflow or submission. A value other than *null* mean that the section will be only available during the specified phase 
* the *sectionType* attribute defines the kind of section that the UI will need to use to interact with the data. See the [documentation about the available values for sectionType provided here](submissionsection-types.md)


## Linked Entities
### config

It returns the endpoint to use to access a detailed, sectionType dependent, configuration to be used in the panel. Such as the list of fields to include in a form based panel, the upload and access condition options for an upload panel, etc.
See the [documentation about the available values for sectionType provided here](submissionsection-types.md)