# WorkflowItem Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/submission/workflowitems**   

Provide access to the workflowitems. It returns the list of existent workflowitems.

Example: to be provided

## Single Workflow Item
**/api/submission/workflowitems/<:id>**

Provide detailed information about a specific workflowitem. The JSON response document is as follow

```json
{
  "id": 1911,
  "lastModified": "2017-06-24T00:40:54.970+0000",
  "step": "editstep",
  "sections": {
  	 "collection": "05457c63-b392-4629-a373-f2d66ee9ee33",
  	 "traditional-page1": {
  	 	"dc.title" : [{value: "Sample Workflow Item"}],
  	 	"dc.contributor.author" : [
  	 		{value: "Bollini, Andrea", authority: "rp00001", confidence: 600}
  	 	]
  	 },
  	 "traditional-page2": {
  	 	"dc.subject" : [{value: "Test"}, {value: "JSON"}, {value: "REST"}],
  	 	"dc.description.abstract" : [
  	 		{value: "This is a long multiline text\n This is a second line of the abstract"}
  	 	]
  	 },
  	 "license": {
  	 	acceptanceDate: "2017-06-24T00:40:54.970+0000",
  	 	url: "http://dspace7.4science.it/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/content"
  	 },
  	 "uploads": [ 
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
	  	 			"name": "openaccess",
	  	 			"groupUUID": "uuid-of-the-anonymous-group"
  	 			},
  	 			{
  	 				"id": 126,
	  	 			"name": "administrator",
	  	 			"groupUUID": "uuid-of-the-administrator-group"
  	 			},
  	 			{
  	 				"id": 127,
	  	 			"name": "embargo",
	  	 			"groupUUID": "1faf7c51-2a14-4826-b0b1-f1c1d2d82dd7",
	  	 			"startDate": "2018-06-24T00:40:54.970+0000"
  	 			},
  	 			{
  	 				"id": 128,
	  	 			"name": "lease",
	  	 			"groupUUID": "38ecd5ae-af12-4144-a276-81532e1679f8",
	  	 			"endDate": "2017-12-24T00:40:54.970+0000"
  	 			}
  	 		]
 		}
 	]
  	},
  	 "cclicense": {
  	 	"image-url": "https://i.creativecommons.org/l/by/4.0/88x31.png",
  	 	"license-uri": "https://creativecommons.org/licenses/by/4.0/"
  	 }
  },
  "type": "workflowitem"
}
```

Similary  to the [Workspace Item](workspaceitems.md) the actual data of the inprogress submission are arranged in *sections* map following the sections configured in the submissionDefinition. This map is *open for extension*, each type of section will expose a different JSON structure. See the [out-of-box submission section types](submissionsection-types.md) page for details. An additional step attribute containing the id of the reached step in the workflow is present

Exposed links:
* step: the workflow step
* collection: the collection where the inprogress submission will be created
* item: the item that hold the submission data
* submissionDefinition: the [submission definition](submissiondefinitions.md) used by this inprogress submission

### Linked entities
#### collection
**/api/workflow/workflowitems/<:id>/collection** (READ-ONLY)

Example: to be provided

It returns the collection where the inprogress submission is occuring. This is a **read-only** endpoint, once the inprogress submission is created the collection cannot be changed directly. Specific section, like the enhanced select-collection section planned for DSpace7 will be able to alter the collection manipulating the *sections* data.

#### item
**/api/workflow/workflowitems/<:id>/item** (READ-ONLY)

Example: to be provided

It returns the backend item holds by the submission. See the [item endpoint for more info](items.md). This is a **read-only** endpoint, once the inprogress submission is created the backend item cannot be changed. Update to the backend item will be reflected automatically on the inprogress submission but are not subject to the submission validation, i.e. they correspond to administrative edits

#### submissionDefinition
**/api/workflow/workflowitems/<:id>/submissionDefinition** (READ-ONLY)

Example: to be provided

It returns the submission definition used by the inprogress submission. See the [submission definitions endpoint for more info](submissiondefinitions.md). This is a **read-only** endpoint. 
The submission definition used by the inprogress submission is derived from the inprogress submission attributes. In the default implementation the definition is derived from the collection where the submission is created and is updated if it changes. 
Please note that this endpoint is not strictly necessary as you can currently retrieve the same definition using [/api/config/submissiondefinitions/search/findByCollection?uuid=<:workspaceitem-collection-uuid>](submissiondefinitions.md#findByCollection) but it allows the client to find the submissionDefinition embedded in the workspaceitem without the need to make a separate call. 
In addition, it allows in future to change the 1:1 association between collections and submissionDefinition without breaking the client

#### workflow step
**/api/workflow/workflowitems/<:id>/step** (READ-ONLY)

It returns the workflow step of the workflow item
See the [workflow steps](workflowsteps.md) endpoint for more info.
This is a **read-only** endpoint

### Search methods
#### findBySubmitter
**/api/workflow/workflowitems/search/findBySubmitter?uuid=<:submitter-uuid>**

It returns the workflowitems created by the specified submitter

## Get Single Workflow Item from Item UUID

**/api/workflow/workflowitems/search/item?uuid=<:item-uuid>**

/api/workflow/workflowitems/search/item?uuid=cd67ce0e-7f9a-42fc-b8e7-c8bb83ef58ca
The item uuid is mandatory

There's always at most one workflow item per Item, so this endpoint will return a single workflow item, not a list

It would respond with:
* 200 OK - Returning the workflow item if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the workflow item
* 204 if the workflow item doesn't exist

## POST Method
To create a workflowitem, i.e. to start a workflow, a workspaceitem must be posted to the workflowitems resource collection endpoint (/api/submission/workflowitems).
The workspaceitem must be supplied as URI in the request body using the text/uri-list content-type
If succeed a 201 code will be returned and the new state of the workflowitem serialized in the body.
If fails the following status code are expected
403 Unauthorized - if the loggedin user is not the submitter of the workspaceitem
422 Unprocessable Entity - if the workspace is not yet ready to be send through the workflow (i.e. there are validation errors)

## Multipart POST Method
Multipart POST request will typically result in the creation of a new file in the section identified by the name of the variable used for the upload (uploads is the default name of the user uploaded content). The process will be managed by the implementation bind with the identified section.
If succeed a 201 code will be returned and the new state of the workflowitem serialized in the body

## DELETE Method
**DELETE /api/submission/workflowitems/<:id>**

Reset a workflow sending back the item to the workspace regardless to the step reached.

If an optional parameter `?expunge=true` is included, the workflow item is deleted instead of being returned to the workflow.

It would respond with:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in as an administrator
* 404 Not found - if the workflow doesn't exist
