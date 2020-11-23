
# WorkspaceItem Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/submission/workspaceitems**   

Provide access to the workspaceitems. It returns the list of existent workspaceitems.

Example: to be provided

## Multipart POST Method

Multipart POST on collections supports two differents operation mode:

* Import from remote resource
* Import from file

In the remote resource scenario, Multipart POST can include a uri-list containing:
* The [external entry value](external-authority-sources.md) whose metadata should be imported

An example curl call:
```
 curl -i -X POST https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems?owningCollection=1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb \
 -H "Content-Type:text/uri-list" \
 --data "https://dspace7.4science.it/dspace-spring-rest/api/integration/externalsources/orcid/entryValues/0000-0002-4271-0436"
```
No confirmation, user has confirmed they want this record, and the previous state of the item is empty

If an external entry value is included, the metadata from this external source should be imported automatically.  
There's no need for a preview of the expected changes similar to the [Metadata Suggestions](metadata-suggestions.md) functionality because
 * The user has already confirmed they want to import this particular record
 * This is a new submission, it starts from an empty item  
 
 
In files scenario, all works in the same way as "remote resource",  but user submits one or more file(s) instead of remote resource id.
There are some constraints to import files:
*   File which doesn't have a valid parser will be discarded.
*   If one or more files contain parseable metadata, the first file that is able to be parsed will be used to prepopulate the metadata. Others will just be saved to the WorkspaceItem
*   All the files attached must have only one entry. Parsing files with more than one entry will result in exception throwing and 422 (Unprocessable Entity) HTTP status.

An example curl call:

    curl --location --request POST 'https://dspace7.4science.it/dspace-spring-rest/api/submission/workspaceitems' 
    --form 'file=@/path/to/bibtex-test.bib' --form 'file=@/path/to/pubmed-test.xml'

    
## Single Workspace Item
**/api/submission/workspaceitems/<:id>**

Provide detailed information about a specific workspaceitem. The JSON response document is as follow
```json
{
  "id": 1911,
  "lastModified": "2017-06-24T00:40:54.970+0000",
  "sections": {
  	 "collection": "05457c63-b392-4629-a373-f2d66ee9ee33",
  	 "traditional-page1": {
  	 	"dc.title" : [{value: "Sample Submission Item"}],
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
  	},
  	 "cclicense": {
  	 	"image-url": "https://i.creativecommons.org/l/by/4.0/88x31.png",
  	 	"license-uri": "https://creativecommons.org/licenses/by/4.0/"
  	 }
  },
  "type": "workspaceitem"
}
```

The actual data of the inprogress submission are arranged in *sections* map following the sections configured in the submissionDefinition. This map is *open for extension*, each type of section will expose a different JSON structure. See the [out-of-box submission section types](submissionsection-types.md) page for details.

Exposed links:
* collection: the collection where the inprogress submission will be created
* item: the item that hold the submission data
* submissionDefinition: the [submission definition](submissiondefinitions.md) used by this inprogress submission

## Multipart POST Method on a single workspaceitem

Multipart POST request will typically result in the creation of a new file in the section identified by the name of the variable used for the upload (uploads is the default name of the user uploaded content). The process will be managed by the implementation bind with the identified section.
If succeed a 201 code will be returned and the new state of the workspaceitem serialized in the body.

An attribute to define the owning collection can be included. If omitted, the first collection the user can submit to will be used.

More informations about multipart post could be found in the previous section "Multipart POST Method".


### Linked entities
#### collection
**/api/submission/workspaceitems/<:id>/collection** (READ-ONLY)

Example: to be provided

It returns the collection where the inprogress submission is occuring. This is a **read-only** endpoint, once the inprogress submission is created the collection cannot be changed directly. Specific section, like the enhanced select-collection section planned for DSpace7 will be able to alter the collection manipulating the *sections* data.

#### item
**/api/submission/workspaceitems/<:id>/item** (READ-ONLY)

Example: to be provided

It returns the backend item holds by the submission. See the [item endpoint for more info](items.md). This is a **read-only** endpoint, once the inprogress submission is created the backend item cannot be changed. Update to the backend item will be reflected automatically on the inprogress submission but are not subject to the submission validation, i.e. they correspond to administrative edits

#### submissionDefinition
**/api/submission/workspaceitems/<:id>/submissionDefinition** (READ-ONLY)

Example: to be provided

It returns the submission definition used by the inprogress submission. See the [submission definitions endpoint for more info](submissiondefinitions.md). This is a **read-only** endpoint. 
The submission definition used by the inprogress submission is derived from the inprogress submission attributes. In the default implementation the definition is derived from the collection where the submission is created and is updated if it changes. 
Please note that this endpoint is not strictly necessary as you can currently retrieve the same definition using [/api/config/submissiondefinitions/search/findByCollection?uuid=<:workspaceitem-collection-uuid>](submissiondefinitions.md#findByCollection) but it allows the client to find the submissionDefinition embedded in the workspaceitem without the need to make a separate call. 
In addition, it allows in future to change the 1:1 association between collections and submissionDefinition without breaking the client

### Search methods
#### findBySubmitter
**/api/submission/workspaceitems/search/findBySubmitter?uuid=<:submitter-uuid>**

It returns the workspaceitem created by the specified submitter

## Get Single Workspace Item from Item UUID

**/api/submission/workspaceitems/search/item?uuid=<:item-uuid>**

/server/api/submission/workspaceitems/search/item?uuid=cd67ce0e-7f9a-42fc-b8e7-c8bb83ef58ca
The item uuid is mandatory

There's always at most one workspace item per Item, so this endpoint will return a single workspace item, not a list

It would respond with:
* 200 OK - Returning the workspace item if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the workspace item
* 204 if the workspace item doesn't exist




