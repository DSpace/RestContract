# WorkspaceItem data of license sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the data about the granted deposit license

```json
{
  	"granted": true,
  	"url": "https://api7.dspace.org/server/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
    "acceptanceDate": "2017-11-20T10:32:42Z"
}
```

* granted: true or false
* url (**READ-ONLY**): the URL where the granted license is visible
* acceptanceDate (**READ-ONLY**): the timestamp of the acceptance as recorded in the dcterms:dataAccepted metadata of the license bitstream. For license granted before than DSpace7 the acceptanceDate will be *null* 

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body 

### Add
To accept a license the client must send a JSON Patch ADD operation to the *granted* path as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/granted", "value": true}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

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
				"granted": false,
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
				"granted": true,
			  	"url": "https://api7.dspace.org/server/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
			    "acceptanceDate": "2017-11-20T10:32:42Z"
			}
		...
	},
	...
}	
```

Please note that according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902) a subsequent add operation on the granted will have the effect to replace the previous granted license with a new one. 
In this case a new bitstream license will be added to the item and the previous license deleted, this mean that the *url* and *acceptedDate* attributes will change accordly.

It is also possible to send a PATH add operation using *false* as value to reject / remove a license.

This use of the add operation to replace the license could be counter intuitive but it is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)
> If the target location specifies an object member that does exist, that member's value is replaced.

### Replace
To accept or reject a license the client can also send a JSON Patch REPLACE operation to the *granted* path as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/granted", "value": true}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/granted", "value": false}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

### Remove
It is possible to remove a previous grant license 
`curl --data '{[ { "op": "remove", "path": "/sections/license/granted"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

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
				"granted": true,
			  	"url": "https://api7.dspace.org/server/api/core/bitstreams/004a297e-fd06-4662-ae51-73e4b7c165c8/content",
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
				"granted": false,
			  	"url": null,
			    "acceptanceDate": null
			}
		...
	},
	...
}	
```
 
### Move, Test & copy
No plan to implement the move, test and copy operations at the current stage