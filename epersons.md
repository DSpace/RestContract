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
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/eperson/epersons/028dcbb8-0da2-4122-a0ea-254be49ca107"
    },
    "groups": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/eperson/epersons/028dcbb8-0da2-4122-a0ea-254be49ca107/groups"
    }
  }
}
```

### Search methods
#### byEmail
**/api/eperson/epersons/search/byEmail?email=<:string>**

The supported parameters are:
* email: mandatory, EPerson's email to search

It returns the EPersonRest instance, if any, matching the user query

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the email parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and users with READ rights on the target EPerson can use the endpoint

#### byMetadata
**GET /api/eperson/epersons/search/byMetadata?query=<:name>**

This supports a basic search in the metadata.
It will search in:
* UUID (exact match)
* first name
* last name
* email address

It returns the list of EPersonRest instances, if any, matching the user query

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the email parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and users with READ rights on the target EPerson can use the endpoint

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
#### This operation can be performed by administrators and by the authenticated user.

To replace the password value, `curl -X PATCH http://${dspace.url}/api/eperson/epersons/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/password", "value": "newpassword"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "password": "oldpassword",
```
the replace operation `[{ "op": "replace", "path": "/password", "value": "newpassword"]` will result in :
```json
  "password": "newpassword",
```
NOTE: The new password is currently returned after an update but this could be revisited later, see [#30]((https://github.com/DSpace/Rest7Contract/issues/30))

The currently authenticated user can modify their EPerson metadata. An administrator can modify any EPerson's metadata.  
This includes, but is not limited to, last name, first name, phone, language

## Create new EPerson

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

## Linked entities
### Groups
**GET /api/eperson/epersons/<:uuid>/groups**

A HAL link to retrieve the eperson groups of an eperson is included.
This will return a pageable list of the groups this person is a direct member of

> TODO: A solution to retrieve direct and indirect groups of an eperson is also required.
> This would use GroupService.allMemberGroupsSet() and is used e.g. when viewing an EPerson as an admin
