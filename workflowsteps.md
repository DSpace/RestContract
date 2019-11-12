# Workflow Steps Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A workflow step represents a single step in the reviewing process, e.g. the accept/reject step.  

All endpoints mentioned here require authentication

## Main Endpoint
**/api/config/workflowsteps**   

Provide access to the configured workflow steps. It returns the list of existent workflow-steps.

## Single Workflow Step Definition
**/api/config/workflowsteps/<:step-name>**

Provide detailed information about a specific workflow step. An example JSON response document:
```json
{
  	"id": "editstep",
    "actions": [
        "approve",
        "reject",
        "edit_metadata"
    ],
  	"type": "workflowstep"
}
```

The **actions** property contains the list of actions the user is authorized to perform in this step:
* The **edit_metadata** action implies the user can use the PATCH on the workflow item's submission sections to edit the metadata.
* Other actions are considered to be buttons with an action sent to REST using a POST to the claimed task