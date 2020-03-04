# Claimed Taks Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/workflow/claimedtasks**   

Not allowed. Only subset of claimed tasks can be retrieved using specific filters see below

## Single Claimed Task
**/api/workflow/claimedtasks/<:id>**

Provide details about a specific task in the pool. The JSON response document is as follow

```json
{
    "id": 1,
    "step": "editstep",
    "action": "editaction",
    "type": "claimedtask"
}
```

Exposed links:
* step: the workflow step
* action: the workflow action
* workflowitem: the workflowitem underlying the task
* owner: the eperson that own the task

### Linked entities
#### workflowitem
**/api/workflow/claimedtasks/<:id>/workflowitem** (READ-ONLY)

It returns the underlying workflowitem holds by the task. See the [workflowitem endpoint for more info](workflowitems.md). This is a **read-only** endpoint, once the task is created the backend workflowitem cannot be changed.

#### owner
**/api/workflow/claimedtasks/<:id>/owner** (READ-ONLY)

It returns the eperson that own the task. See the [eperson endpoint for more info](epersons.md). This is a **read-only** endpoint, once the task is claimed the owner cannot be changed. To unclaim a task see the DELETE method below

#### workflow step
**/api/workflow/claimedtasks/<:id>/step** (READ-ONLY)

It returns the workflow step currently assigned to the task.
See the [workflow steps](workflowsteps.md) endpoint for more info.
This is a **read-only** endpoint

#### workflow action
**/api/workflow/claimedtasks/<:id>/action** (READ-ONLY)

It returns the workflow action currently assigned to the task.
See the [workflow actions](workflowactions.md) endpoint for more info.
This is a **read-only** endpoint, the [POST to the claimed task](#post-method-single-resource-level) is used to perform the action

### Search methods
#### findByUser
**/api/workflow/claimedtasks/search/findByUser?uuid=<:user-uuid>**

It returns the tasks claimed by the specified user

## POST Method (collection level)
The creation of claimed tasks is managed by the underline workflow system. No methods are exposed to manually trigger such creation to avoid workflow hjack and inconsistency.

## POST Method (single resource level)
To perform a claimed task a POST request must be issued against the single claimed task URL
/api/workflow/claimedtasks/:id
using http request parameters to instruct the workflow action class about the specific command to perform approve, reject, add a score, etc. In some case other parameters must be specified too as for instance the reason parameter for the reject action

| action | command options | parameter to trigger the command | additional parameters |
| --- | --- | --- | --- |
| editaction | approve | submit\_approve=true | _none_ |
| editaction | reject | submit\_reject=true | reason=<text to send to the submitter> |
| finaleditaction | approve | submit\_approve=true | _none_ |
| selectrevieweraction | choose a reviewer | submit\_select\_reviewer\_<uuid>=true | no extra parameters needed but the primary parameter name define the selected eperson |
| scorereviewaction |record the score | submit_score=true | score=<int value of the score> |

## Return code
* 204 No content is returned if the request succeed
* 403 Not authorized if the task cannot be performed by the logged in user
* 404 if the task is not longer available
* 422 validation errors (e.g. no "reason" parameter provided)

## DELETE Method 
To unclaim a task it is sufficient to issue a DELETE request

## Multipart POST Method
Not allowed
