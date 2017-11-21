# WorkspaceItem data of submission-form sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the metadata collected for a specific [submissionform](submissionforms.md) it is a json object with the following structure

```json
{
  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
  "dc.contributor.author" : [
  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600}
  ]
}
```

The attribute name is the metadata key using the '.' dot separator (i.e. dc.title, dc.contributor.author, etc.) the value is an array, sorted by the metadatavalue.place column, containing
* value: the textual value of the metadata
* authority: the authority key if any associated
* confidence: the level of confidence for the authority if any, -1 otherwise
* language: the iso code associated with the textual value if any

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

### Add
To add a new value to an **existent metadata** the client must send a JSON Patch ADD operation as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/<:metadata>/-", "value": {value: "...", authority: "...", confidence: 600, language: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

it is also possible to insert the new metadata in a specific position

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/<:metadata>/<:idx-zero-based>", "value": {value: "...", authority: "...", confidence: 600, language: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

for example, starting with the following document  
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600}
		  ]
		},
		...
	},
	...
}	
```

the following request 
`curl --data '{[ { "op": "add", "path": "/sections/traditional-page1/dc.contributor.author/-", "value": {value: "Another, Author"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

will result in
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ]
		},
		...
	},
	...
}	
```

and an additional call as follow
`curl --data '{[ { "op": "add", "path": "/sections/traditional-page1/dc.contributor.author/1", "value": {value: "Insert, Test", authority: "rp00002", confidence: 600}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

will get the final document
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ]
		},
		...
	},
	...
}	
```

Please note that according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902) to initialize a new metadata in the section the add operation must receive an array of values and it is not possible to add a single value to the not yet initialized "/sections/<:name-of-the-form>/<:metadata>/-" path.
For example the following call will add the first keyword to our previous example

`curl --data '{[ { "op": "add", "path": "/sections/traditional-page1/dc.subject", "value": [{value: "keyword1"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

will get the final document
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ],
		  "dc.subject" : [{value: "keyword1"}]
		},
		...
	},
	...
}	
```

it is also possible to initialize or **replace** the whole metadata values sending multiple objectvalue in the call
`curl --data '{[ { "op": "add", "path": "/sections/traditional-page1/dc.subject", "value": [{value: "keyword1"},{value: "keyword2"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ],
		  "dc.subject" : [{value: "keyword1"}, {value: "keyword2"}]
		},
		...
	},
	...
}	
```

This use of the add operation to replace the list of values of a metadata could be counter intuitive but it is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)
> If the target location specifies an object member that does exist, that member's value is replaced.

the specification explicitly say that trying to send the request against the *- array position index* will create a sub-array element, see the [specification Appendix A.16](https://tools.ietf.org/html/rfc6902#page-18)   

### Remove
It is possible to remove a specific metadatavalue
`curl --data '{[ { "op": "remove", "path": "/sections/traditional-page1/dc.subject/0"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

trasforming
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ],
		  "dc.subject" : [{value: "keyword1"}, {value: "keyword2"}]
		},
		...
	},
	...
}	
```
in
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ],
		  "dc.subject" : [{value: "keyword2"}]
		},
		...
	},
	...
}	
```
or removing all the metadata values for a specific metadata key
`curl --data '{[ { "op": "remove", "path": "/sections/traditional-page1/dc.contributor.author"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.subject" : [{value: "keyword2"}]
		},
		...
	},
	...
}	
```
 
### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To change the title of the previous example
`curl --data '{[ { "op": "replace", "path": "/sections/traditional-page1/dc.title/0", "value": {value: "Modified title", language:"en"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Modified title", language: "en"}],
		  "dc.subject" : [{value: "keyword2"}]
		},
		...
	},
	...
}	
```

It is also possible to change only the language of the existent title 
`curl --data '{[ { "op": "replace", "path": "/sections/traditional-page1/dc.title/0/language", "value": "it"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Modified title", language: "it"}],
		  "dc.subject" : [{value: "keyword2"}]
		},
		...
	},
	...
}	
```

### Move
It is possible to rearrange the metadata values using the move operation. For instance to put the 3rd author of the following workspaceitem 
```json
{
	id: 1,
	type: "workspaceitem",
	sections:
	{
		"traditional-page1":
		{
		  "dc.title" : [{value: "Sample Submission Item", language: "en"}],
		  "dc.contributor.author" : [
		  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600},
		  	 		{value: "Insert, Test", authority: "rp00002", confidence: 600},
		  	 		{value: "Another, Author", confidence: -1}
		  ],
		  "dc.subject" : [{value: "keyword2"}]
		},
		...
	},
	...
}	
```

as first author you need to run
`curl --data '{[ { "op": "move", "from": "/sections/traditional-page1/dc.contributor/2", "path": "/sections/traditional-page1/dc.contributor/0"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`


### Test & copy
No plan to implement the test and copy operation at the current stage