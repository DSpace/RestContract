# Researcher profiles Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This repository allows to manage the profile item linked to a particular eperson. The specific type of item 
with which the profile is modeled depends on the dspace configuration (by default it is the Person type). If the particular dspace instance doesn't handle the "profile" 
type then these endpoints are disabled.

## Get all Profile
**GET /api/eperson/profiles/**

This operation is not currently supported.

## Single Profile
**GET /api/eperson/profiles/<:eperson-uuid>**

Provide information about a specific profile. Only the Administrators or the owner eperson can access this endpoint.

The JSON response document is as follow

```json
{
  "id": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
  "visible": true,
  "orcid": "0000-0002-8310-6788",
  "orcidSynchronization": {
    "mode": "MANUAL",
    "publicationsPreference": "ALL",
    "projectsPreference": "ALL",
    "profilePreferences": ["BIOGRAPHICAL", "IDENTIFIERS"]
  },
  "type": "profile"
}
```

Attributes:
* id: the UUID of the EPerson who owns the profile. This also corresponds to the ID of the profile.
* visible: this property provides a quick way to add/remove the Anonymous READ policy. When set to "true", then the Anonymous READ Policy is added (and the profile becomes accessible to anonymous users). When set to "false", then the Anonymous READ policy is removed (and the profile is no longer accessible anonymously). Keep in mind, this setting only impacts the Anonymous READ policy. So, if other custom READ policies exist on the profile, those policies will take effect when "visible=false".
* orcid: the orcid id 
* orcidSynchronization: contains the orcid synchronization mode and preferences 
* type: the resource type (profile)

The orcid and orcidSynchronization attributes are not showed if the given profile is not linked to any ORCID account.

Exposed links:
* item: the item with which the profile is modeled
* eperson: the eperson related to the profile

It would respond with:
* 200 OK - Returning the profile if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the profile
* 404 Not found - if the profile or the eperson doesn't exist

## Linked entities

### Item
**GET /api/eperson/profiles/<:eperson-uuid>/item**

Returns the item that model the profile of the eperson with the given uuid. The JSON response document is as follow

```json
{
  "id": "d0601db5-5ceb-4f8d-8762-bd5b3b97845d",
  "uuid": "d0601db5-5ceb-4f8d-8762-bd5b3b97845d",
  "name": null,
  "handle": "123456789/511",
  "metadata": {
    "dspace.object.owner": [{
        "value": "Mortimer Smith",
        "language": null,
        "authority": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
        "confidence": 600,
        "place": 0
      }],
    "dc.date.accessioned": [{
        "value": "2020-07-20T15:31:11Z",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }],
    "dc.date.available": [{
        "value": "2020-07-20T15:31:11Z",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }],
    "dc.description.provenance": [{
        "value": "Made available in DSpace on 2020-07-20T15:31:11Z (GMT). No. of bitstreams: 0",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      }],
    "dc.identifier.uri": [{
        "value": "http://localhost:4000/handle/123456789/511",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }],
    "dspace.entity.type": [{
        "value": "Person",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2020-07-20T16:02:19.608+0000",
  "type": "item"
}
```
It would respond with:
* 200 OK - Returning the item if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the item
* 404 Not found - if the item doesn't exist

### EPerson
**GET /api/eperson/profiles/<:eperson-uuid>/eperson**

Returns the eperson owning the profile. The JSON response document is as follow

```json
{
  "id": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
  "uuid": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
  "name": "msmith@samltest.id",
  "handle": null,
  "metadata": {
    "eperson.firstname": [{
        "value": "Mortimer",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }],
    "eperson.lastname": [{
        "value": "Smith",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }]
  },
  "netid": "msmith@samltest.id",
  "lastActive": "2019-10-25T08:05:46.530+0000",
  "canLogIn": true,
  "email": "msmith@samltest.id",
  "requireCertificate": false,
  "selfRegistered": false,
  "type": "eperson"
}
```

It would respond with:
* 200 OK - Returning the eperson if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the eperson
* 404 Not found - if the eperson doesn't exist

## Creating a new Profile
**POST /api/eperson/profiles?eperson=<:eperson-uuid>**

Create a new profile for the specified person or for the current user. This action can only be done by the EPerson themselves or an Administrator.

Parameters:
* eperson-uuid (optional): the uuid of eperson for which the profile is to be created; if not specified, the eperson is the authenticated one

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 Unprocessable Entity - if the specified eperson does not exist or if for the specified eperson a profile already exists

In case of response with status 201 the endpoint returns the newly created resource, while in case of 422 it returns the resource that generated the conflict.

To create a new profile use 
```bash
curl -i -X POST ${dspace-url}/api/eperson/profiles?eperson=eb645ef8-1373-41eb-bf67-6afcea7e2069 -H "Content-Type:application/json"
```

## Claim an existing Profile
**POST /api/eperson/profiles?eperson=<:eperson-uuid>**

Modify the profile item specified in the request content to make the given eperson its owner. The content-type is uri-list with the url of the item's profile to be claimed.
This action can only be done by the EPerson themselves or an Administrator.

Parameters:
* eperson-uuid (optional): the uuid of eperson for which the profile is to be claimed; if not specified, the eperson is the authenticated one

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not Found - if the specified eperson does not exist
* 422 Unprocessable Entity - if the specified eperson has already a profile or if the profile to be claimed is already owned

An example curl call:
```bash
 curl -i -X POST ${dspace-url}/api/eperson/profiles?eperson=eb645ef8-1373-41eb-bf67-6afcea7e2069 -H "Content-Type:text/uri-list" \
 --data ${dspace-url}/api/core/items/cec6ee1b-7730-44da-a224-a7c5af63f821
```

## Hide/unhide a profile
**PATCH /api/eperson/profiles/<:eperson-uuid>**

This operation allow to change the visibility of one profile. This action can only be done by the EPerson themselves or an Administrator.
A value of true means the Profile is given Anonymous READ access. A value of false, means the Profile is not accessible Anonymously.
Using the ResourcePolicies endpoint directly is possible to give more complex permissions.

To hide or unhide a profile use 
```bash
curl -i -X PATCH ${dspace-url}/api/eperson/profiles/eb645ef8-1373-41eb-bf67-6afcea7e2069 --data '[ { "op": "replace", "path": "/visible", "value": true }]' -H "Content-Type:application/json"
```

Status codes:

* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if a profile for the specified eperson does not exist

If the operation succeed the endpoint returns the updated resource.

## Modify ORCID synchronization preferences
**PATCH /api/eperson/profiles/<:eperson-uuid>**

This operation allow to change the ORCID synchronization mode and preferences of a given profile. Only an administrator can modify the profile of another eperson.
To do this, REPLACE operations must be used with one of the following paths:
* **/orcid/mode** - to update synchronization mode; allowed values are 'BATCH', 'MANUAL'
* **/orcid/publications** - to update the preference relative to the publications synchronization; allowed values are 'DISABLED', 'ALL'
* **/orcid/projects** - to update the preference relative to the projects synchronization; allowed values are 'DISABLED', 'ALL'
* **/orcid/profile** - to update the preference relative to the profile synchronization; allowed values are a 'BIOGRAPHICAL' (to synchronize other names, country and keywords) and 'IDENTIFIERS' (to synchronize external ids and urls). It is possible to specify multiple values using ',' as separator.


To modify the synchronization preferences use
```bash
curl -i -X PATCH ${dspace-url}/api/eperson/profiles/eb645ef8-1373-41eb-bf67-6afcea7e2069 --data '[ { "op": "replace", "path": "/orcid/mode", "value": "MANUAL" }]' -H "Content-Type:application/json"
```

Status codes:

* 200 OK - if the operation succeed
* 400 Bad Request - if the given profile is not already linked to an ORCID account
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if a profile for the specified eperson does not exist
* 422 Unprocessable entity - if the provided path or value is not valid

## Connect a profile with ORCID
**PATCH /api/eperson/profiles/<:eperson-uuid>**

This operation allow to connect an ORCID account with a given profile. Only an administrator can modify the profile of another eperson.
To do this, ADD operations must be used with one of the following path:
* **/orcid** - the allowed value is the authorization code for an ORCID iD and 3-legged access token.

```bash
curl -X PATCH http://${dspace.server.url}/api/eperson/profiles/<:id-eperson> -H "* Content-Type: application/json" -d '[{ "op": "add", "path": "/orcid", value: <code> }]'
```

Status codes:

* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if a profile for the specified eperson does not exist
* 422 Unprocessable entity - if the given code is not valid
* 500 Internal server error - if there are integration configuration errors or if the orcid service is down

## Disconnect a profile from ORCID
**PATCH /api/eperson/profiles/<:eperson-uuid>**

This operation allow to disconnect an ORCID account from a given profile. Only an administrator can modify the profile of another eperson.
To do this, REMOVE operations must be used with one of the path **/orcid**

```bash
curl -X PATCH http://${dspace.server.url}/api/eperson/profiles/<:id-eperson> -H "* Content-Type: application/json" -d '[{ "op": "remove", "path": "/orcid" }]'
```
Status codes:

* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if a profile for the specified eperson does not exist

## Delete a profile
**DELETE /api/eperson/profiles/<:eperson-uuid>**

Delete the profile related to the given eperson. This action can only be done by the EPerson themselves or an Administrator.
The type of deletion depends on a configuration property. It can be a soft deletion, where only the link between the EPerson and Profile object is removed 
(so the Profile object remains). Or, it can be a hard deletion, where the Profile object is permanently deleted.


To delete a profile use
```bash
curl -i -X DELETE ${dspace-url}/api/eperson/profiles/eb645ef8-1373-41eb-bf67-6afcea7e2069
```

Return codes:
* 204: if the delete succeeded (including the case of no-op) 
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
