# Submission-panels Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/configuration/submission-definitions**   

Provide access to the configured submission definitions. It returns the list of existent submission-definitions.

Example: to be provided

## Single Submission-panels 
**/api/configuration/submission-definitions/<:definition-name>**

Provide detailed information about a specific submission-definition. The JSON response document is as follow
```json
{
  "name": "traditional",
  "panels": [
  {
  	id: "id-of-the-input-form-page",
  	header: "First page",
  	mandatory: true,
  	type: "inputform",
  	scope: null,
  	_links: {
  		"configuration" : "<dspace-url>/configuration/inputforms/<:id-of-the-input-form-page>" 
  	}
  },
  {
    id: "id-of-the-input-form-page",
  	header: "Second page",
  	mandatory: true,
  	type: "inputform",
  	scope: null,
  	_links: {
  		"configuration" : "<dspace-url>/configuration/inputforms/<:id-of-the-input-form-page>" 
  	}
  },
  {
    id: "id-of-the-upload-panel",
  	header: "Files and access condition",
  	mandatory: true,
  	type: "uploadWithEmbargo",
  	scope: null
  },
  {
  	id: "id-of-the-deposit-license-panel",
  	header: "Deposit license",
  	mandatory: true,
  	type: "license",
  	scope: null
  },
  {
  	id: "id-of-the-cc-license-panel",
  	header: "Creative Commons",
  	mandatory: false,
  	type: "cclicense",
  	scope: null
  },
  ...  
  ],
  "isDefault": true
}

```

Exposed links:
* collections: list of collections that explicitly use such submission-definition

## Search methods
### findByCollection
**/api/configuration/submission-definitions/search/findByCollection?uuid=<:collection-uuid>**

It returns the submission definition that apply to a specific collection eventually fallback to the default configuration 

## Linked entities
### collections
**/api/configuration/submission-definitions/<:definition-name>/collections**

It returns the list of collection that make an explicit use of the submission-definition. If a collection doesn't specify the submission-definition to be used the default mapping apply but this collection is not included in the list returned by this method
