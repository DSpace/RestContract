# EPersons Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/epersons**

## Single EPerson
**GET /api/eperson/epersons/<:uuid>**

```json
{
  "id": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "uuid": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "name": "user@institution.edu",
  "handle": null,
  "metadata": {
      "eperson.firstname": [
        {
          "value": "John",
          "language": null,
          "authority": "",
          "confidence": -1,
          "place": 0
        }
      ],
      "eperson.language": [
        {
          "value": "en",
          "language": null,
          "authority": "",
          "confidence": -1,
          "place": 0
        }
      ],
      "eperson.lastname": [
        {
          "value": "Doe",
          "language": null,
          "authority": "",
          "confidence": -1,
          "place": 0
        }
      ]
  },
  "netid": null,
  "lastActive": "2019-09-25T15:59:28.000+0000",
  "canLogIn": true,
  "email": "user@institution.edu",
  "requireCertificate": false,
  "selfRegistered": true,
  "type": "eperson",
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/eperson/epersons/028dcbb8-0da2-4122-a0ea-254be49ca107"
    },
    "groups": {
      "href": "https://demo.dspace.org/server/api/eperson/epersons/028dcbb8-0da2-4122-a0ea-254be49ca107/groups"
    }
  }
}
```

## Search methods
### byEmail
**/api/eperson/epersons/search/byEmail?email=<:string>**

The supported parameters are:
* email: mandatory, EPerson's email address to search

It returns the EPersonRest instance, if any, matching the user query

Return codes:
* 200 OK - if the search operation succeeds and an EPerson with the given email address exists
* 204 OK - if the search operation succeeds, but an EPerson with the given email address does not exist
* 400 Bad Request - if the email parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and users with READ rights on the target EPerson can use the endpoint

### byMetadata
**GET /api/eperson/epersons/search/byMetadata?query=<:string>**

This supports a basic search across all EPerson accounts via their metadata.
It will search in:
* UUID (exact match)
* first name
* last name
* email address

It returns the list of EPersonRest instances, if any, matching the user query

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the 'query' parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or Community/Collection administrators can use this endpoint.

### isNotMemberOf
**GET /api/eperson/epersons/search/isNotMemberOf?group=<:uuid>&query=<:string>**

This supports a basic search across all EPersons which are not already a member of the provided Group (in the 'group' parameter). Therefore it searches EPersons _not already listed_ on the `/api/eperson/groups/<:uuid>/epersons` endpoint for the provided group.
It will search in:
* UUID (exact match)
* first name
* last name
* email address

It returns the list of EPersonRest instances, if any, matching the user query

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the 'group' or 'query' parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or Community/Collection administrators can use this endpoint.

## Patch operations

EPerson metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

Additional properties can be modified via Patch as described below.

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

#### These operations can be performed by administrators.

To replace the certificate required value, `curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/certificate", "value": "true|false"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "requireCerticate": true,
```
the replace operation `[{ "op": "replace", "path": "/certificate", "value": "false"]` will result in :
```json
  "requireCerticate": false,
```

To replace the canLogin value, `curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/canLogin", "value": "true|false"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "canLogIn": true,
```
the replace operation `[{ "op": "replace", "path": "/canLogin", "value": "false"` will result in :
```json
  "canLogIn": false,
```
To replace the netid value, `curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/netid", "value": "newNetId"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "netid": "oldNetId",
```
the replace operation `[{ "op": "replace", "path": "/netid", "value": "newNetId"]` will result in :
```json
  "netid": "newNetId",
```

To replace the email value, `curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/email", "value": "new@email"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "email": "old@email",
```
the replace operation `[{ "op": "replace", "path": "/email", "value": "new@email"]` will result in :
```json
  "email": "new@email",
  ```  
  
#### These operations can be performed by administrators and by the authenticated user.

The currently authenticated user can modify their EPerson metadata. An administrator can modify any EPerson's metadata.  
This includes, but is not limited to, last name, first name, phone, language
  
### Add
The add operation allows adding or replacing information in the EPerson. Password changes are currently required to use "add" requests.
  
#### These operations can be performed by administrators and by the authenticated user.

To add/replace the password value while authenticated, use
`curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "add", "path": "/password", "value": {"new_password": "newpassword", "current_password":"oldpassword"}}]'`.  
To add/replace the password value based on a registration token, use
`curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson>?token=<:token> -H "Content-Type: application/json" -d '[{ "op": "add", "path": "/password", "value": {"new_password": "newpassword"}}]'`.  
The operation requires an Authorization header or a token.

If the provided current_password is wrong, a response with status 403 Forbidden is returned.

For example, starting with the following eperson field data:
```json
 "password": "oldpassword",
```
the replace operation `[{ "op": "add", "path": "/password", "value": {"new_password": "newpassword", "current_password":"oldpassword"}}]` will result in :
```json
  "password": "newpassword",
```
Status codes:
* 422 Unprocessable Entity - If the provided password not respects the rules configured in the regular expression

NOTE: The new password is currently returned after an update but this could be revisited later, see [#30](https://github.com/DSpace/Rest7Contract/issues/30)

## Create new EPerson (requires admin permissions)
**POST /api/eperson/epersons**

To create a new EPerson, perform a post with the JSON below to the epersons endpoint when logged in as admin.

```json
{
  "name": "user@institution.edu",
  "metadata": {
    "eperson.firstname": [
      {
        "value": "John",
        "language": null,
        "authority": "",
        "confidence": -1
      }
    ],
    "eperson.lastname": [
      {
        "value": "Doe",
        "language": null,
        "authority": "",
        "confidence": -1
      }
    ]
  },
  "canLogIn": true,
  "email": "user@institution.edu",
  "requireCertificate": false,
  "selfRegistered": true,
  "type": "eperson"
}
```

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 Unprocessable Entity - if the email address was omitted or already exists

## Create new EPerson based on registration token
**POST /api/eperson/epersons?token=<:token>**

To create a new EPerson, perform a post with the JSON below to the epersons endpoint while including a token.
The token will be sent via Email from the [Create new EPerson registration](epersonregistrations.md#create-new-eperson-registration).

```json
{
  "metadata": {
    "eperson.firstname": [
      {
        "value": "John",
        "language": null,
        "authority": "",
        "confidence": -1
      }
    ],
    "eperson.lastname": [
      {
        "value": "Doe",
        "language": null,
        "authority": "",
        "confidence": -1
      }
    ]
  },
  "canLogIn": true,
  "requireCertificate": false,
  "type": "eperson"
}
```

The "eperson.firstname" and "eperson.lastname" metadata are mandatory. The phone number, language, … are optional metadata.  
The email property can be set, but would need to be identical to the value from the registration.  
The selfRegistered property can be set, but would need to be true

Status codes:
* 201 Created - if the operation succeed
* 400 Bad Request - if the email address didn't match the token or already exists. If the token doesn't exist or is expired
* 401 Unauthorized - if the token doesn't allow you to create this account
* 422 Unprocessable Entity - If the provided password not respects the rules configured in the regular expression

## Linked entities
### Groups
**GET /api/eperson/epersons/<:uuid>/groups**

A HAL link to retrieve the eperson groups of an eperson is included.
This will return a pageable list of the groups this person is a direct member of

> TODO: A solution to retrieve direct and indirect groups of an eperson is also required.
> This would use GroupService.allMemberGroupsSet() and is used e.g. when viewing an EPerson as an admin
