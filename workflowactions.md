# Workflow Actions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A workflow action represents a single action in the reviewing process, e.g. the accept/reject action.  

All endpoints mentioned here require authentication, but no specific permissions.

## Main Endpoint
**/api/config/workflowactions**   

The main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single Workflow Action Definition
**/api/config/workflowactions/<:action-name>**

Provide detailed information about a specific workflow action. An example JSON response document:
```json
{
  	"id": "editaction",
  	"options": [
  	    "approve",
  	    "reject",
  	    "edit_metadata"
  	],
  	"type": "workflowaction"
}
```

The **options** property contains the list of actions the user is authorized to perform in this step:
* The **edit_metadata** option implies the user can use the PATCH on the workflow item's submission sections to edit the metadata.
* Other options are considered to be command options sent to REST using a [POST to the claimed task](claimedtasks.md#post-method-single-resource-level)