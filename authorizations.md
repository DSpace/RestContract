# Authorizations Endpoints
[Back to the list of all defined endpoints](endpoints.md)

An authorization is the representation of some rights that are available to a specific user (eperson) on a defined object, eventually the whole repository (site object).
All the authorizations are always explicitly listed regardless to how they are grant, by direct policies via groups' membership or open to everyone (anonymous users).
Please note that these endpoints are all READ-ONLY, authorizations are granted and withdrawn as result of business processes, configurations and resource policies.

## Main Endpoint
**/api/authz/authorizations**

As we don't have yet an use case to iterate over all the existent authorizations the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Single Authorization
**/api/authz/authorizations/<:id>**

Provide detailed information about a specific authorization. The JSON response document is as follow

```json
{
  "id": [eperson-uuid_]feature-id_restobjectcategory.restobjectname_object-id,
  "type": "authorization"
}
```

Attributes
* id: the id of the authorization resource is defined by the combination of the eperson uuid (if not null), the feature id and the rest object category and name (combined with a dot) and (uu)id joined with an underscore 

Exposed links:
* eperson: link to the eperson that the authorization belong to. Can be null for authorizations grant to unlogged users
* feature: link to the feature enabled by this authorization
* object: link to the rest object where this authorization apply. Not limited to DSpace objects, potentially any BaseObjectRest (i.e. an addressable rest object with an Id) can be used

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated and the policy is not related to the ANONYMOUS user (eperson null)
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and the user mentioned in the authorization can access
* 404 Not found - if the authorization doesn't exist (or was already deleted)

### Search methods
#### object
**/api/authz/authorizations/search/object?uri=<:uri>[&eperson=<:uuid>&feature=<:string>]**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uri: mandatory, the object to use for the authorization check. The full URI of the rest resource must be specified, i.e. https://{dspace.url}/api/core/communities/{uuid}
* eperson: optional, the uuid of the eperson to evaluate for authorization. If not specified authorization of anonymous users will be returned
* feature: optional, limit the returned authorization to the specified feature (this provide an alternative to codify the authorization id rule on the client side)

It returns the list of matching authorizations. Please note that all the matching authorizations available for the requested user will be returned including the one available to anonymous users. There is no need on the client side to combine the authorizations of the current, logged-in, user with the authorizations of anonymous users. 

Return codes:
* 200 OK - if the operation succeed. It could eventually result in an empty list
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated and an eperson is specified
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and the user specified in the eperson parameter can access

#### objects
**/api/authz/authorizations/search/objects?uuid=<:uuid>&type=<:type>[&eperson=<:uuid>&feature=<:string>]**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, repeatable. Represents the list of objects to be used for the authorization check. For each of them, the UUID must be specified.2
* type: mandatory. Represents the type of resource(s) (i.e. "core.items", "workflow.workflowitems",...)  on which authorizations are checked.
* eperson: optional, the uuid of the eperson to evaluate for authorization. If not specified authorization of anonymous users will be returned
* feature: optional, repeatable. Represents the list of features. Limits the returned authorizations to the specified features (this provide an alternative to codify the authorization id rule on the client side)

It returns the list of matching authorizations. Please note that all the matching authorizations available for the requested user will be returned including the one available to anonymous users. There is no need on the client side to combine the authorizations of the current, logged-in, user with the authorizations of anonymous users. 

Return codes:
* 200 OK - if the operation succeed. It might result in an empty list
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated and an eperson is specified
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and the user specified in the eperson parameter can access

## Other HTTP verbs than GET
No other verbs are supported right now, these endpoints are all READ-ONLY. Authorizations cannot be grant or revoke directly, they are generated by the system via interaction with other endpoint such as the resourcepolicies, the submission, the workflow, etc.

## Linked entities
### EPerson
**/api/authz/authorizations/<:string>/eperson**

Return the eperson linked by this authorization

Return codes:
* 200 Ok - if the operation succeed and an eperson is set for this policy
* 204 No content - if the authorization is related to anonymous users
* 401 Unauthorized - if you are not authenticated and the authorization is not related to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Authorization endpoint
* 404 Not found - if the authorization doesn't exist

### Object
**/api/authz/authorizations/<:string>/object**

Return the resource where the authorization is scoped.

Return codes:
* 200 Ok if the operation succeed
* 401 Unauthorized - if you are not authenticated and the authorization is not related to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Authorization endpoint
* 404 Not found - if the authorization doesn't exist

### Feature
**/api/authz/authorizations/<:string>/feature**

Return the feature resource that the user can access thanks to this authorization.

Return codes:
* 200 Ok if the operation succeed
* 401 Unauthorized - if you are not authenticated and the authorization is not related to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions. See the requirement in the GET Single Authorization endpoint
* 404 Not found - if the authorization doesn't exist
