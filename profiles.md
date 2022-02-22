# Researcher profiles Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Single Profile
**GET /api/cris/profiles/<:eperson-uuid>**

Provide information about a specific profile. Only the Administrators or the owner eperson can access this endpoint.

The JSON response document is as follow

```json
{
  "id": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
  "visible": true,
  "type": "profile"
}
```

Attributes:
* id: the profile id that corresponds to the id of the eperson associated
* visible: indicates whether the profile is public (visible: true) or private (otherwise)
* type: the resource type (profile)

Exposed links:
* item: the item with which the profile is modeled
* eperson: the eperson related to the profile

It would respond with:
* 200 OK - Returning the profile if there's a match
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to view the profile
* 404 Not found - if the profile doesn't exist

## Linked entities

### Item
**GET /api/cris/profiles/<:eperson-uuid>/item**

Returns the item that model the profile of the eperson with the given uuid. The JSON response document is as follow

```json
{
  "id": "d0601db5-5ceb-4f8d-8762-bd5b3b97845d",
  "uuid": "d0601db5-5ceb-4f8d-8762-bd5b3b97845d",
  "name": null,
  "handle": "123456789/511",
  "metadata": {
    "cris.owner": [{
        "value": "Mortimer Smith",
        "language": null,
        "authority": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
        "confidence": 600,
        "place": 0
      }],
    "cris.sourceId": [{
        "value": "eb645ef8-1373-41eb-bf67-6afcea7e2069",
        "language": null,
        "authority": null,
        "confidence": -1,
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
    "relationship.type": [{
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
**GET /api/cris/profiles/<:eperson-uuid>/eperson**

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
**POST /api/cris/profiles?eperson=<:eperson-uuid>**

Create a new profile for the specified person or for the current user. Only an administrator can create a profile for another eperson.

Parameters:
* eperson-uuid (optional): the uuid of eperson for which the profile is to be created; if not specified, the eperson is the authenticated one

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 409 Conflict - if for the specified eperson a profile already exists
* 422 Unprocessable Entity - if the specified person does not exist

In case of response with status 201 the endpoint returns the newly created resource, while in case of 409 it returns the resource that generated the conflict.

To create a new profile use 
```bash
curl -i -X POST ${dspace-url}/api/cris/profiles?eperson=eb645ef8-1373-41eb-bf67-6afcea7e2069 -H "Content-Type:application/json"
```

## Hide/unhide a profile
**PATCH /api/cris/profiles/<:eperson-uuid>**

This operation allow to change the visibility of one profile. Only an administrator can modify the profile of another eperson.

To hide or unhide a profile use 
```bash
curl -i -X PATCH ${dspace-url}/api/cris/profiles/eb645ef8-1373-41eb-bf67-6afcea7e2069 --data '[ { "op": "replace", "path": "/visible", "value": true }]' -H "Content-Type:application/json"
```

Status codes:

* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if a profile for the specified eperson does not exist

If the operation succeed the endpoint returns the updated resource.

## Delete a profile
**DELETE /api/cris/profiles/<:eperson-uuid>**

Delete the profile related to the given eperson. Only an administrator can delete the profile of another eperson.

To delete a profile use
```bash
curl -i -X DELETE ${dspace-url}/api/cris/profiles/eb645ef8-1373-41eb-bf67-6afcea7e2069
```

Return codes:
* 204: if the delete succeeded (including the case of no-op) 
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
