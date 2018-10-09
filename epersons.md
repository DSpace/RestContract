# EPersons Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/eperson/epersons**  

## Single EPerson
**/api/eperson/epersons/<:uuid>**

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