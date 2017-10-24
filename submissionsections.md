# Submission-Sections Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A submission-section represents a single section in the submission process. Depending on its type, it may include a configurable input form (if type='submission-form'). However, not all sections are currently configurable, and other types of steps include file upload, embargo/access rights, creative commons licensing, etc.

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

## Linked Entities
### config

It returns the endpoint to use to access a detailed, panel-type dependend, configuration to be used in the panel. Such as the list of fields to include in a form based panel, the upload and access condition options for an upload panel, etc.