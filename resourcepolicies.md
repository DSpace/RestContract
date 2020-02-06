# Resource Policies Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/authz/resourcepolicies**   

As we don't have yet an use case to iterate over all the existent resource policies the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single Resource Policy
**/api/authz/resourcepolicies/<:id>**

Provide detailed information about a specific resource policy. The JSON response document is as follow

```json
{
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : null,
  "endDate" : null,
  "type": "resourcepolicy"
}
```

Attributes
* name: a short name associated to describe the policy
* description: a description for the policy, i.e. the reason for the policy
* policyType: a classification of the policy meanly from the process that has set it. One of 
    * TYPE_SUBMISSION: a policy in place during the submission
    * TYPE_WORKFLOW: a policy in place during the approval workflow
    * TYPE_INHERITED: a policy that has been inherited from a container (the collection)
    * TYPE_CUSTOM: a policy defined by the user during the submission or workflow phase
    * null: if the information is not available
* action: the action grant by the policy. One of READ, WRITE, ADD, REMOVE, ADMIN, DELETE, WITHDRAWN_READ, DEFAULT_BITSTREAM_READ, DEFAULT_ITEM_READ as defined in the Constants class
* startDate: the first day of validity of the policy, if null the policy is assumed valid from ever. The date will be in the format YYYY-MM-DD
* endDate: the last day of validity of the policy, if null the policy is assumed valid for ever. The date will be in the format YYYY-MM-DD

Exposed links:
* eperson: link to the eperson that have permission grant by the policy
* group: link to the group that have permission grant by the policy
* resource: link to the resource subject to the policy

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated. Please note that this also apply to resource policy related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators, users with ADMIN right on the target resource, users mentioned in the policy (eperson or member of the group) can access the resourcepolicy
* 404 Not found - if the resourcepolicy doesn't exist (or was already deleted)

### Search methods
#### resource
**/api/authz/resourcepolicies/search/resource?uuid=<:uuid>[&action=<:string>]**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the resource object of the policy
* action: optional, limit the returned policies to the specified action (see GET description for allowed values)

It returns the list of matching resource policies

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and users with ADMIN right on the target resource can use the endpoint

#### eperson
**/api/authz/resourcepolicies/search/eperson?uuid=<:uuid>[&resource=<:uuid>]**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the eperson that benefit of the policy
* resource: optional, limit the returned policies to the ones related to the specified resource

It returns the list of explicit matching resource policies, no inherited or broader resource policies will be included in the list nor policies derived by groups' membership

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or the user specified in the uuid parameter can use the endpoint

#### group
**/api/authz/resourcepolicies/search/group?uuid=<:uuid>[&resource=<:uuid>]**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the group that benefit of the policy
* resource: optional, limit the returned policies to the ones related to the specified resource

It returns the list of explicit matching resource policies, no inherited or broader resource policies will be included in the list nor policies derived by groups' membership

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated. Please note that this also apply to search related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or users member of the group specified in the uuid parameter

## Creating a resource policy
**POST /api/authz/resourcepolicies?resource=<:uuid>&[eperson=<:uuid>|group=<:uuid>]**

Request body:
```json
{
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : null,
  "endDate" : null,
  "type": "resourcepolicy"
}
```

The parameters are:
* resource: mandatory, the uuid of the resource target of the policy
* eperson: the uuid of the eperson that will be grant of the permission. Exactly one of eperson or group is required
* group: the uuid of the group that will be grant of the permission. Exactly one of eperson or group is required

The json body must be valid that mean
- type must be resourcepolicy
- policyType must be one of the value specified in the GET description
- action must be one of the value specified in the GET description
- startDate / endDate if present must respect the format specified in the GET description

Return codes:
* 200 OK - if the operation succeed, the created resourcepolicy is returned
* 400 Bad Request - if both the group and the eperson uuid parameters are specified or missing or aren't uuid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can create resourcepolicy


## Patch operations

### Add
The add operation allows to initialize or replace information with new one.

To set a startDate
`curl --data '[ { "op": "add", "path": "/startDate", "value": "2019-10-31" }]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/authz/resourcepolicies/${resourcepolicy-id}`

```json
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : null,
  "endDate" : null,
  "type": "resourcepolicy"
```

the add operation will result in:

```json
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2019-10-31",
  "endDate" : null,
  "type": "resourcepolicy"
```

To set a name and a description
`curl --data '[ { "op": "add", "path": "/name", "value": "my name" }, { "op": "add", "path": "/description", "value": "my description"}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/authz/resourcepolicies/${resourcepolicy-id}`


```json
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2019-10-31",
  "endDate" : null,
  "type": "resourcepolicy"
```

the add operation will result in:

```json
  "id": 2844,
  "name": "my name",
  "description": "my description",
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2019-10-31",
  "endDate" : null,
  "type": "resourcepolicy"
```

Return codes, see also general [return codes for PATCH requests](patch.md#error-codes):
* 200 Ok if the path operation succeed. The updated resource policy is included in the response
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or user with ADMIN permission over the target resource can use the endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)

### Remove
With the `remove` operation, you can delete:
- startDate
- endDate
- name
- description

the other properties cannot be nullified.

To remove an embargo you can for instance use the following
`curl --data '[ { "op": "remove", "path": "/startDate" }]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/authz/resourcepolicies/${resourcepolicy-id}`

that will transform

```json
  "id": 2844,
  "name": "my name",
  "description": "my description",
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2019-10-31",
  "endDate" : null,
  "type": "resourcepolicy"
```

in 

```json
  "id": 2844,
  "name": "my name",
  "description": "my description",
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : null,
  "endDate" : null,
  "type": "resourcepolicy"
```

Return codes, see also general [return codes for PATCH requests](patch.md#error-codes):
* 200 Ok if the path operation succeed. The updated resource policy is included in the response
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or user with ADMIN permission over the target resource can use the endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To change the startDate
`curl --data '[ { "op": "replace", "path": "/startDate", "value": "2020-01-01" }]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/authz/resourcepolicies/${resourcepolicy-id}`

For example, starting with the following item data:

```json
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2019-10-31",
  "endDate" : null,
  "type": "resourcepolicy"
```

the replace operation will result in:

```json
  "id": 2844,
  "name": null,
  "description": null,
  "policyType": "TYPE_SUBMISSION",
  "action": "READ",
  "startDate" : "2020-01-01",
  "endDate" : null,
  "type": "resourcepolicy"
```

Return codes, see also general [return codes for PATCH requests](patch.md#error-codes):
* 200 Ok if the path operation succeed. The updated resource policy is included in the response
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or user with ADMIN permission over the target resource can use the endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)

## Deleting a resource policy
**DELETE /api/authz/resourcepolicies/<:id>**

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or user with ADMIN permission over the target resource can use the endpoint
* 404 Not found - if the item doesn't exist (or was already deleted)


## Linked entities
### EPerson
**/api/authz/resourcepolicies/<:id>/eperson**

Return the eperson linked by this resource policy

The recipient of the policy, eperson or group, cannot be modified. If you need to do so please delete the policy and create a new one.

Return codes:
* 200 Ok - if the operation succeed and an eperson is set for this policy
* 204 No content - if the operation succeed but no eperson is set for this policy
* 401 Unauthorized - if you are not authenticated and the policy is not related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Resource Policy endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)


### Group
**/api/authz/resourcepolicies/<:id>/group**

Return the group linked by this resource policy

The recipient of the policy, eperson or group, cannot be modified. If you need to do so please delete the policy and create a new one.

Return codes:
* 200 Ok if the operation succeed and a group is linked to this resource policy
* 204 No content - if the operation succeed but no group is set for this policy
* 401 Unauthorized - if you are not authenticated and the policy is not related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Resource Policy endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)

### Resource
**/api/authz/resourcepolicies/<:id>/resource**

Return the resource target of this resource policy.

The resource target cannot be modified. If you need to do so please delete the policy and create a new one.

Return codes:
* 200 Ok if the operation succeed
* 401 Unauthorized - if you are not authenticated and the policy is not related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Resource Policy endpoint
* 404 Not found - if the resource policy doesn't exist (or was already deleted)
