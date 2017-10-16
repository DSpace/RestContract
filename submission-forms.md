# Submission-Forms Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/config/submission-forms**   

Provide access to the configured submission-forms. It returns the list of existent submission-forms.
Please note that from DSpace 7 the submission-form concept has been introduced splitting the previous configuration file named input-forms.xml by page. Each submission-form describes a single page of inputs, combining different pages together is done with the [submission-definition](submission-definitions.md), aka the item-submission.xml configuration file 

Example: to be provided

## Single Submission-Form 
**/api/config/submission-forms/<:form-name>**

Provide detailed information about a specific input-form. The JSON response document is as follow
```json
{
  "name": "traditional-page1",
  "fields": [
  		{
  			label: "Authors",
  			repeatable: false,
  			mandatory: null,
  			hints: "Enter the names of the authors of this item.",
  			input: {
  				type: "name"
  			},
  			selectableMetadata: [
  				{
  					"metadata": "dc.contributor.author",
  					"authority": "SolrAuthorAuthority",
  					"closed": false
  				}
  			],
  			scope: null, 
  			visibility: {
  				main: null,
  				other: null
  			},
  			typeBind: [],
  			languageCodes: []
  		},
  		{
			label: "Title",
  			repeatable: false,
  			mandatory: "You must enter a main title for this item.",
  			hints: "Enter the main title of the item.",
  			input: {
  				type: "onebox",
  				regex: null
  			},
  			selectableMetadata: [
  				{
  					metadata: "dc.title"
  				}
  			],
  			scope: null, 
  			visibility: {
  				main: null,
  				other: null
  			},
  			typeBind: [],
  			languageCodes: [
  				{
  					display: "English",
  					code: "en_US"
				},
				{
  					display: "Italian",
  					code: "it_IT"
				}
  			]
  		},
  		...
  ]
}

```

it is important to note that the field definition contains special attribute that in an ideal HAL representation should be replaced with links but for simplicity we have preferred to expose as string
* authority: the name of the authority used to retrieve value for the input [see authorities](authorities.md) 
* metadata: the key of the metadata field to use to store the input

The *scope* attribute can be null or one of the values: workflow or submission. A value other than null mean that the visibility of the field in the specified scope is defined by the value of the attribute *visibility.main* and *visibility.other* in the other scope. *Null* mean that the field will be visible in both the submission and workflow with the visibility specified in *visibility.main* 
The visibility attributes can assume one of the following values
* *null* : editable
* *readonly*: visible but not alterable
* *hidden*: not visible