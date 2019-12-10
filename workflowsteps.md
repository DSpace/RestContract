# Workflow Steps Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A workflow step represents a single step in the reviewing process, e.g. the accept/reject step.  

All endpoints mentioned here require authentication, but no specific permissions.

## Main Endpoint
**/api/config/workflowsteps**   

Provide access to the configured workflow steps. It returns the list of configured workflow-steps.

## Single Workflow Step Definition
**/api/config/workflowsteps/<:step-name>**

Provide detailed information about a specific workflow step. An example JSON response document:
```json
{
  	"id": "editstep",
  	"type": "workflowstep",
    "_links": {
      "workflowactions": {
        "href": "https://dspace7-entities.atmire.com/rest/api/config/workflowsteps/<:step-name>/workflowactions"
      }
    },
    "_embedded": {
      "workflowactions": 
        [
          {
            "id": "editaction",
            "options": [
                "approve",
                "reject",
                "edit_metadata"
            ],
            "type": "workflowaction"
          }
        ]
    }
}
```

It includes the list of workflow actions used in the workflow step. See [Workflow Actions Endpoints](workflowactions.md) for more details