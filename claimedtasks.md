# Claimed Tasks Endpoints
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

#### findAllByItem
**/api/workflow/claimedtasks/search/findAllByItem?uuid=<:item-uuid>**
Accessible only by Admin
It returns all the claimed tasks related to the specified item

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
**/api/workflow/claimedtasks/search/findByItem?uuid=<:item-uuid>**
It returns, if any, the single task related to the specified item claimed by the current user

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the item object

Return codes:
* 200 OK - if the operation succeed
* 204 No Content - if there is no claimed task for the specified item and the current user
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the provided uuid cannot be resolved to an item regardless to the item status

## POST Method
To create a claimedtask, i.e. to claim a pooltask. 
The pooltask must be supplied as URI in the request body using the text/uri-list content-type
If successful a 201 code will be returned along with the new claimedtask. The pooltask will also be removed.

An example curl call:
```
curl -i -X POST https://dspace7.4science.it/dspace-spring-rest/api/workflow/claimedtasks
\ -H "Content-Type:text/uri-list" \
--data "https://dspace7.4science.it/dspace-spring-rest/api/workflow/pooltasks/1"
```

Return codes:
201 Created - if the operation succeed
401 Unauthorized - if you are not authenticated
403 Forbidden -  if the loggedin user is not the reviewer of the pooltask
422 Unprocessable Entity - if the pooltask provided doesn't exist

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
