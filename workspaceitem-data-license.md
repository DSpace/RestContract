# WorkspaceItem data of license sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the data about the granted deposit license

```json
{
  	"url": "https://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
    "acceptanceDate": "2017-11-20T10:32:42Z"
}
```

* url (**READ-ONLY**): the URL where the accepted license is visible
* acceptanceDate: the timestamp of the acceptance as recorded in the dcterms:dataAccepted metadata of the license bitstream. For license accepted before than DSpace7 the acceptanceDate will be *null* 

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body 

### Add
To accept a license the timestamp of the acceptance the client must send a JSON Patch ADD operation as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/acceptanceDate", "value": "YYYY-MM-DDTHH:MM:SSZ"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

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
		"license":
			{
			  	"url": null,
			    "acceptanceDate": null
			}
		...
	},
	...
}	
```

the following request 
`curl --data '{[ { "op": "add", "path": "/sections/license/acceptanceDate", "value": "2017-11-20T10:32:42Z"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

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
			  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600}
			  ]
			},
		"license":
			{
			  	"url": "https://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
			    "acceptanceDate": "2017-11-20T10:32:42Z"
			}
		...
	},
	...
}	
```

Please note that according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902) a subsequent add operation on the acceptanceDate will have the effect to replace the previous granted license with a new one.

This use of the add operation to replace the license could be counter intuitive but it is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)
> If the target location specifies an object member that does exist, that member's value is replaced.

### Remove
It is possible to remove a previous grant license 
`curl --data '{[ { "op": "remove", "path": "/sections/license/acceptanceDate"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

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
			  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600}
			  ]
			},
		"license":
			{
			  	"url": "https://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
			    "acceptanceDate": "2017-11-20T10:32:42Z"
			}
		...
	},
	...
}	
```

back in
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
		"license":
			{
			  	"url": null,
			    "acceptanceDate": null
			}
		...
	},
	...
}	
```

This will works also in the case where the license was granted (url != null) but the acceptanceDate was not recorded (< DSpace 7).

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation without a previous accepted license must return an error. See [general errors on PATCH requests](patch.md)

`curl --data '{[ { "op": "replace", "path": "/sections/license/acceptanceDate", "value": "2017-11-21T12:31:14Z"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

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
		"license":
			{
			  	"url": "https://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/new-uuid/content",
			    "acceptanceDate": "2017-11-21T12:31:14Z"
			}
		...
	},
	...
}	
```
Please note that a new bitstream license will be added to the item and the previous license deleted, this mean that the *url* attribute will change accordly.
 
### Move, Test & copy
No plan to implement the move, test and copy operations at the current stage