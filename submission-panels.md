# Submission-panels Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/config/submission-panels**   

Provide access to the configured submission panels. It returns the list of existent submission-panels.

Example: to be provided

## Single Submission-Definition
**/api/config/submission-panels/<:panel-name>**

Provide detailed information about a specific submission-panel. The JSON response document is as follow
```json
{
  	id: "id-of-the-submission-form-page",
  	header: "First page",
  	mandatory: true,
  	type: "submission-form",
  	scope: null,
  	_links: {
  		"config" : "<dspace-url>/config/submission-forms/<:id-of-the-submission-form-page>" 
  	}
}
```

## Linked Entities
### config

It returns the endpoint to use to access a detailed, panel-type dependend, configuration to be used in the panel. Such as the list of fields to include in a form based panel, the upload and access condition options for an upload panel, etc.