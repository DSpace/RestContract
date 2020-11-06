# Pool Taks Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/workflow/pooltasks**   

Not allowed. Only subset of pool tasks can be retrieved using specific filters see below

## Single Pool Task
**/api/workflow/pooltasks/<:id>**

Provide details about a specific task in the pool. The JSON response document is as follow

```json
{
    "id": 1,
    "step": "editstep",
    "action": "claimaction",
    "type": "pooltask"
}
```

Exposed links:
* step: the workflow step
* action: the workflow action
* workflowitem: the workflowitem underlying the task
* eperson: the eperson that can claim the task
* group: the group that can claim the task

### Linked entities
#### workflowitem
**/api/workflow/pooltasks/<:id>/workflowitem** (READ-ONLY)

It returns the underlying workflowitem holds by the task. See the [workflowitem endpoint for more info](workflowitems.md). This is a **read-only** endpoint, once the task is created the backend workflowitem cannot be changed.

#### eperson
**/api/workflow/pooltasks/<:id>/eperson** (READ-ONLY)

It returns the eperson that can claim the task. See the [eperson endpoint for more info](epersons.md). This is a **read-only** endpoint, once the task is created the backend eperson cannot be changed.

#### group
**/api/workflow/pooltasks/<:id>/group** (READ-ONLY)

It returns the group of epersons that can claim the task. See the [group endpoint for more info](epersongroups.md). This is a **read-only** endpoint, once the task is created the backend group cannot be changed.

#### workflow step
**/api/workflow/pooltasks/<:id>/step** (READ-ONLY)

It returns the workflow step currently assigned to the task.
See the [workflow steps](workflowsteps.md) endpoint for more info.
This is a **read-only** endpoint

#### workflow action
**/api/workflow/pooltasks/<:id>/action** (READ-ONLY)

It returns the workflow action currently assigned to the task.
See the [workflow actions](workflowactions.md) endpoint for more info.
This is a **read-only** endpoint.

### Search methods
#### findByUser
**/api/workflow/workflowitems/search/findByUser?uuid=<:user-uuid>**

It returns the tasks available for the specified user

## POST Method (collection level)
The creation of pool tasks is managed by the underline workflow system. No methods are exposed to manually trigger such creation to avoid workflow hjack and inconsistency.

## POST Method (single resource level)
To claim a pool task a POST request must be issued against the single pool task URL
/api/workflow/pooltasks/:id

204 No content is returned if the request succeed
403 Not authorized if the task cannot be claimed by the logged in user
404 if the task is not longer available

## DELETE Method 
Not allowed. To reset a workflow it is possible to issue a DELETE against the [workflowitem endpoint](workflowitem.md)

## Multipart POST Method
Not allowed
