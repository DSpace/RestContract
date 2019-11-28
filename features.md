# Features Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A feature is the representation of a business goal used in the [Authorization endpoint](authorizations.md) to declare what an user can do on a specific object.

## Main Endpoint
**/api/authz/features**

List all the available features in the system. Access is restricted to system administrators.

Parameters:
* page, size [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access

## Single Feature
**/api/authz/features/<:string>**

Provide detailed information about a specific feature. Access is restricted to system administrators. The JSON response document is as follow

```json
{
  "id": "withdrawItem",
  "description": "The feature allows to withdrawn an item from the repository without deleting it. The restoreItem feature allow to undo the process",
  "resourcetypes": [
  	"item"
  ],
  "type": "feature"
}
```

Attributes
* id: the id of the feature is an unique shortname
* description: an human readable description of the feature purpose 
* resourcetypes: an array of types of objects where this feature apply in the textual, singular, lowercase, form. See org.dspace.core.Constants.typeText can be one of site, community, collection, item, bundle, bitstream, eperson, group, workspaceitem, workflowitem, pooltask, claimedtask

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access
* 404 Not found - if the authorization doesn't exist (or was already deleted)

### Search methods
#### resourcetype
**/api/authz/features/search/resourcetype?type=<:string>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* type: see org.dspace.core.Constants.typeText can be one of site, community, collection, item, bundle, bitstream, eperson, group, workspaceitem, workflowitem, pooltask, claimedtask

It returns the list of features that apply to the specified type. 

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the type parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access 