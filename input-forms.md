# Input-forms Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/configuration/input-forms**   

Provide access to the configured input-forms. It returns the list of existent input-forms.

Example: to be provided

**/api/configuration/value-pairs**   

Provide access to the configured value-pairs. It returns the list of existent value-pairs.

Example: to be provided

## Single Input-Form 
**/api/configuration/input-forms/<:form-name>**

Provide detailed information about a specific input-form. The JSON response document is as follow
```json
{
  "name": "traditional",
  "pages": [
  {
  	header: "First page",
  	mandatory: true,
  	fields: [
  		{
  			label: "Authors",
  			repeatable: false,
  			mandatory: null,
  			hints: "Enter the names of the authors of this item.",
  			input: {
  				type: "name"
  			}
  			scope: null, 
  			visibility: {
  				main: null,
  				other: null
  			},
  			typeBind: []
  		},
  		{
			label: "Title",
  			repeatable: false,
  			mandatory: "You must enter a main title for this item.",
  			hints: "Enter the main title of the item.",
  			input: {
  				type: "onebox",
  				regex: null,
  				closedVocabulary: false  				
  			}
  			scope: null, 
  			visibility: {
  				main: null,
  				other: null
  			},
  			typeBind: []
  		},
  		...
  	]
  },
  ...  
  ],
  "isDefault": true
}

```

Exposed links:
* value-pairs: list of value-pairs used by the specific input-form
* collections: list of collections that explicitly use such input-form

Inside the field definition we will have links if appropriate to
* metadata: the link to the metadataregistry entry
* authority: a definition of the authority used by the field (mainly to retrieve the autocomplete/lookup endpoint)
* vocabulary: the link to the vocabulary xml file
* value-pair: the list of entries for dropdown, checkbox, etc.
 
## Search methods
### findByCollection
**/api/configuration/input-forms/search/findByCollection?uuid=<:collection-uuid>**

It returns the input form that apply to a specific collection eventually fallback to the default configuration
 
## Linked entities
### Value pairs
**/api/configuration/input-forms/<:form-name>/value-pairs**

It returns the list of value-pairs used by the specific input form

### collections
**/api/configuration/input-forms/<:form-name>/collections**

It returns the list of collection that make an explicit use of the input-form. If a collection doesn't specify the input-form to be used the default mapping apply but this collection is not included in the list returned by this method

### metadata
** /api/core/metadatafields/<:metadata-uuid> **

It returns the metadataregistry entry associated with the field

### authority
**/api/submission/authority/<:authority-name> **

It returns the authority used by the field/metadata

### vocabulary
** /api/configuration/vocabulary/<vocabylary-name> ** 

It returns the vocabulary used by the field in its raw XML format

### value-pair
**/api/configuration/value-pairs/<:value-pair-name>**

It returns the value pair used by the field

```json
{
  "name": "common_types",
  "entries": [
		{
      		displayValue: {"en":"Animation"}
      		storedValue: "Animation",
      		storedLang: "en"
    	},
    	{
    		displayValue: {"en":"Article"}
      		storedValue: "Article",
      		storedLang: "en"
    	},
    	...
  ]
}
```


