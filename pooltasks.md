# Pool Tasks Endpoints
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

#### findAllByItem
**/api/workflow/pooltasks/search/findAllByItem?uuid=<:item-uuid>**
Accessible only by Admin
It returns all the pool tasks related to the specified item

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the item object

Return codes:
* 200 OK - if the operation succeed. This include the case of no matching tasks where a 0-size page json representation is returned.
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only users with ADMIN right can use the endpoint
* 422 Unprocessable Entity - if the provided uuid cannot be resolved to an item regardless to the item status

#### findByItem
**/api/workflow/pooltasks/search/findByItem?uuid=<:item-uuid>**
It returns, if any, the single pooltask related to the specified item

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the item object

Return codes:
* 200 OK - if the operation succeed
* 204 No Content - if there is no pool task for the specified item and the current user
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the provided uuid cannot be resolved to an item regardless to the item status

## POST Method (collection level)
The creation of pool tasks is managed by the underline workflow system. No methods are exposed to manually trigger such creation to avoid workflow hjack and inconsistency.

## POST Method (single resource level)
Not allowed. To claim a pool task, please POST against the [claimed tasks](claimedtasks.md#post-method) endpoint.

## DELETE Method 
Not allowed. To reset a workflow it is possible to issue a DELETE against the [workflowitem endpoint](workflowitem.md)

## Multipart POST Method
Not allowed
