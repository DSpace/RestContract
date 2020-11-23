# WorkspaceItem data of upload sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the data about the user uploaded files

```json
{
	"files": [ 
  	 	{
  	 		metadata: {
  	 			"dc.title" : [{value: "sample_file.pdf"}],
  	 			"dc.description" : [{value: "Description of the sample file"}]
  	 		},
  	 		"sizeBytes": 8528,
			"checkSum": {
			    "checkSumAlgorithm": "MD5",
			    "value": "9d8f0f9e369cf12159d47c146c499cf4"
			},
  	 		"url": "http://dspace7.4science.it/api/core/bitstreams/00001abf-b2e0-477a-99de-104db7cb6469/content",
  	 		"accessConditions": [
  	 			{
  	 				"id": 123,
	  	 			"name": "openaccess"
  	 			},
  	 			{
  	 				"id": 126,
	  	 			"name": "administrator"
  	 			},
  	 			{
  	 				"id": 127,
	  	 			"name": "embargo",
	  	 			"startDate": "2018-06-24T00:40:54.970+0000"
  	 			},
  	 			{
  	 				"id": 128,
	  	 			"name": "lease",
	  	 			"endDate": "2017-12-24T00:40:54.970+0000"
  	 			}
  	 		]
 		}
 	]
}
```
the files attribute contains the list of user uploaded file in the section. For each file the following attributes exist
* metadata: the map of the metadata assigned to the specific file with [the same structure](workspaceitem-data-metadata.md) used in the submission-form sectionType for the item metadata
* sizeBytes (**READ-ONLY**): the size of the received file as calculated on the server at the receiving time 
* checkSum (**READ-ONLY**): the checksum details (algorithm and value) of the received file as calculated on the server at the receiving time
* url (**READ-ONLY**): from where the file can be downloaded
* accessConditions: an array of all the policies that has been applied by the user to the file. 

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body. See also the [General rules for the Patch operation](patch.md) for more details.

### Add
The add operation is used to add new metadata or access condition to already uploaded files. To upload a completely new file a [POST Multipart request must be issued against the workspaceitems endpoint, see here](workspaceitems.md).

#### Metadata
To add a value to an **existent metadata** the client must send a JSON Patch ADD operation as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/metadata/<:metadata>/-", "value": {value: "...", authority: "...", confidence: 600, language: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

it is also possible to insert the new metadata in a specific position

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/metadata/<:metadata>/<:idx-zero-based>", "value": {value: "...", authority: "...", confidence: 600, language: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

To **initialize or replace completely** the values of a specific metadata

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/metadata/<:metadata>", "value": [{value: "...", authority: "...", confidence: 600, language: ".."}]}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`
 
for instance the call
`curl --data '{[ { "op": "add", "path": "/sections/uploads/files/0/metadata/dc.title", "value": [{value: "MyFile.pdf"}]}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

will set the title of the first uploaded file to MyFile.pdf returning the following json document
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
		"uploads":
		{	
			"files": [ 
	  	 	{
	  	 		metadata: {
	  	 			"dc.title" : [{value: "MyFile.pdf"}],
	  	 			"dc.description" : [{value: "Description of the sample file"}]
	  	 		},
	  	 		"sizeBytes": 8528,
				"checkSum": {
				    "checkSumAlgorithm": "MD5",
				    "value": "9d8f0f9e369cf12159d47c146c499cf4"
				},
	  	 		"url": "http://dspace7.4science.it/api/core/bitstreams/00001abf-b2e0-477a-99de-104db7cb6469/content",
	  	 		"accessConditions": [
	  	 			{
	  	 				"id": 123,
		  	 			"name": "openaccess"
	  	 			},
	  	 			{
	  	 				"id": 126,
		  	 			"name": "administrator"
	  	 			},
	  	 			{
	  	 				"id": 127,
		  	 			"name": "embargo",
		  	 			"startDate": "2018-06-24T00:40:54.970+0000"
	  	 			},
	  	 			{
	  	 				"id": 128,
		  	 			"name": "lease",
		  	 			"endDate": "2017-12-24T00:40:54.970+0000"
	  	 			}
	  	 		]
	 		}
 		]
		}
	}
}
```

you can find more example to manipulate metadata in the documentation about [the submission-form sectionType](workspaceitem-data-metadata.md) you will only need to use the *path* to the file'metadata as specified above, i.e. adding /files/<:idx-file>/metadata/

#### Access Condition
To add an access condition the client must send a JSON Patch ADD operation as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/-", "value": {name: "...", endDate: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to **replace completely** the access conditions applied to a specific file

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessCondition", "value": [{name: "...", endDate: ".."}]}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>

The exact attributes that the access condition to add must have depend on the name attribute that will identify the access condition configuration as exposed by the [submissionupload configuration endpoint](submissionuploads.md).
For instance the following request is valid
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/-", "value": {name: "openaccess"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

instead the request
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/-", "value": {name: "embargo"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

will be rejected because a startDate attribute is also expected when an embargo is applied (according to the example configuration listed in this doc for the [submissionupload configuration endpoint](submissionuploads.md). The right request will be
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/-", "value": {name: "embargo", startDate: "2018-12-31"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

Please note that doesn't make sense to add an access condition in a specific index and, to simplify the client implementation it was assumed that the accessCondition array is never *null* but eventually an empty array so that you call always append new AccessCondition to the path "/sections/<:name-of-the-form>/files/<:file-idx>/accessCondition/-".

### Remove
It is possible to remove an uploaded file specifying its index in the path of a remove operation 
`curl --data '{[ { "op": "remove", "path": "/sections/uploads/files/<:idx-file>"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

#### Metadata
Similarly to what has been described for the add operation you can look to the documentation about [the submission-form sectionType](workspaceitem-data-metadata.md) for example about how to remove a metadata or a single metadata value for the file. You will only need to use the *path* to the file'metadata as specified above, i.e. adding /files/<:idx-file>/metadata/

#### Access Condition
To remove a previous applied access condition it is sufficient to invoke a remove operation on the accessCondition position path, i.e.
`curl --data '{[ { "op": "remove", "path": "/sections/uploads/files/<:idx-file>/accessConditions/<:access-idx>"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

the following request will reset the access condition of the specified file to the empty array
`curl --data '{[ { "op": "remove", "path": "/sections/uploads/files/<:idx-file>/accessConditions"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation without a previous value must return an error. See [general errors on PATCH requests](patch.md)

#### Metadata
Similarly to what has been described for the add operation you can look to the documentation about [the submission-form sectionType](workspaceitem-data-metadata.md) for example about how to replace a metadata or a single metadata value for the file. You will only need to use the *path* to the file'metadata as specified above, i.e. adding /files/<:idx-file>/metadata/

#### Access Condition
You can replace an existent access condition with a new one or update some settings of an existent access condition using the replace operation. The new settings must be valid for the selected access condition (name) according to the [submissionupload configuration endpoint](submissionuploads.md).

To replace an access condition with a new one
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/0", "value": {name: "embargo", startDate: "2018-12-31"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to update the embargo start date
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/0/startDate", "value": "2019-12-31"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to transform an existent *openaccess* access condition to an *administrator* access condition
`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/files/<:file-idx>/accessConditions/0/name", "value": "administrator"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

please note that the above works only because of openaccess and administrator have the same settings needs (no need of addition information). Indeed, the backend is expected to remove the existent policy and create a new policy. If the settings of the previous and new access condition differs the request must fail with a 422 error code

### Move
The move operation is allowed to the files index path to change the order of the uploaded file. 

The following request will move the 3rd uploaded file in the first position
`curl --data '{[ { "op": "add", "from":"/sections/<:name-of-the-form>/files/2", "path": "/sections/<:name-of-the-form>/files/0}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

#### Metadata
Similarly to what has been described for the add operation you can look to the documentation about [the submission-form sectionType](workspaceitem-data-metadata.md) for example about how to move a metadata value for the file in the case the metadata is repeatable. You will only need to use the *path* to the file'metadata as specified above, i.e. adding /files/<:idx-file>/metadata/

### Test & copy
No plan to implement the test and copy operations at the current stage
