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
  "metadata": [
    {
      "key": "eperson.firstname",
      "value": "John",
      "language": null
    },
    {
      "key": "eperson.lastname",
      "value": "Doe",
      "language": null
    },
    {
      "key": "eperson.phone",
      "value": "",
      "language": null
    },
    {
      "key": "eperson.language",
      "value": "en",
      "language": null
    }
  ],
  "netid": null,
  "lastActive": null,
  "canLogIn": true,
  "email": "user@institution.edu",
  "requireCertificate": false,
  "selfRegistered": true,
  "type": "eperson",
  "_links": {
    "self": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/eperson/epersons/028dcbb8-0da2-4122-a0ea-254be49ca107"
    }
  }
}
```

## Patch operations

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)


To replace the certificate required value, `curl -X PATCH http://${dspace.url}/api/epersons/eperson/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/certificate", "value": "true|false"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "requireCerticate": true,
```
the replace operation `[{ "op": "replace", "path": "/certificate", "value": "false"]` will result in :
```json
  "requireCerticate": false,
```

To replace the canLogin value, `curl -X PATCH http://${dspace.url}/api/epersons/eperson/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/canLogin", "value": "true|false"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "canLogIn": true,
```
the replace operation `[{ "op": "replace", "path": "/canLogin", "value": "false"` will result in :
```json
  "canLogIn": false,
```
To replace the netid value, `curl -X PATCH http://${dspace.url}/api/epersons/eperson/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/netid", "value": "newNetId"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "netid": "oldNetId",
```
the replace operation `[{ "op": "replace", "path": "/netid", "value": "newNetId"]` will result in :
```json
  "netid": "newNetId",
```

To replace the password value, `curl -X PATCH http://${dspace.url}/api/epersons/eperson/<:id-eperson> -H "Content-Type: application/json" -d '[{ "op": "replace", "path": "/password", "value": "newpassword"]'`.  The operation also requires an Authorization header.

For example, starting with the following eperson field data:
```json
 "password": "oldpassword",
```
the replace operation `[{ "op": "replace", "path": "/password", "value": "newpassword"]` will result in :
```json
  "password": "newpassword",
```
Note: The new password is currently returned after an update but this could be revisited later, see [#30]((https://github.com/DSpace/Rest7Contract/issues/30))

## Create new EPerson

**POST /api/eperson/epersons**

To create a new EPerson, perform a post with the JSON below to the epersons endpoint when logged in as admin.

```json
{
  "name": "user@institution.edu",
  "metadata": [
    {
      "key": "eperson.firstname",
      "value": "John"
    },
    {
      "key": "eperson.lastname",
      "value": "Doe"
    }
  ],
  "canLogIn": true,
  "email": "user@institution.edu",
  "requireCertificate": false,
  "selfRegistered": true,
  "type": "eperson"
}
```
