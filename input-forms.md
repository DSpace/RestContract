# Input-forms Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/configuration/input-forms**   

Provide access to the configured input-forms. It returns the list of existent input-forms.
Please note that starting from DSpace 7 each input-form will describe a single page of inputs, combining different pages togheter is done with the [submission-definition](submission-panels.md), aka the item-submission.xml configuration file 

Example: to be provided

## Single Input-Form 
**/api/configuration/input-forms/<:form-name>**

Provide detailed information about a specific input-form. The JSON response document is as follow
```json
{
  "name": "traditional",
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