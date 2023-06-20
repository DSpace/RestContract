# EPerson groups Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/groups**

## Single EPerson Group
**GET /api/eperson/groups/<:uuid>**

```json
{
  "id": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
  "uuid": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
  "name": "Administrator",
  "handle": null,
  "metadata": {
  },
  "permanent": true,
  "type": "group",
  "_links": {
    "subgroups": {
      "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/subgroups"
    },
    "epersons": {
      "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/epersons"
    },
    "self": {
      "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc"
    }
  }
}
```

The "permanent" attribute will be "true" if the group is a required system group (e.g. the 'Administrator' group).  
A permanent group cannot be removed or have its name altered.

## Create new EPerson Group

**POST /api/eperson/groups**

To create a new EPerson Group, perform a post with the JSON below to the epersons endpoint when logged in as admin.

```json
{
  "name": "New Group 1",
  "metadata": {
      "dc.description": [
        {
          "value": "Test group",
          "language": null,
          "authority": "",
          "confidence": -1
        }
      ]
  }
}
```

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 Unprocessable Entity - if the name was omitted or already exists, if permanent was set to true

## Deleting an EPerson Group

**DELETE /api/eperson/groups/<:uuid>**

Deletes the EPerson Group

Status codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only general admins can delete a group
* 404 Not found - if the Group doesn't exist (or was already deleted)
* 422 Unprocessable Entity - if you attempt to delete a permanent group or a group that has a parent (community admins, collection submitters, ... these have separate endpoints to delete them)

## Patch operations
**PATCH /api/eperson/groups/<:uuid>**

EPerson Group metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

Additional properties can be modified via Patch as described below.

### Replace
The replace operation allows to replace *existent* information with new one.
The only property which can be modified is the group name.

To change the name of an EPerson Group, use
`curl --data '[ { "op": "replace", "path": "/name", "value": "New Name"}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api//eperson/groups/${group-uuid}`.

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only general admins can patch a group
* 404 Not found - if the Group doesn't exist (or was already deleted)
* 422 Unprocessable Entity - if the name cannot be modified (e.g. if permanent was set to true or the group is part of a DSpace Object)

## Sub Groups in a single parent EPerson Group

### Get Sub Groups in a single parent EPerson Group

**GET /api/eperson/groups/<:uuid>/subgroups**

Retrieves direct child groups of the given group.
Requesting indirect child groups is not supported

Sample:
* GET /rest/api/eperson/groups/3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/subgroups

```json
{
    "_embedded": {
      "groups": [
        {
          "id": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
          "uuid": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
          "name": "Administrator",
          "handle": null,
          "metadata": {},
          "permanent": true,
          "type": "group",
          "_links": {
            "subgroups": {
              "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/subgroups"
            },
            "epersons": {
              "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/epersons"
            },
            "self": {
              "href": "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc"
            }
          }
        }
      ]
    },
    "_links": {
      "self": {
        "href": "https://api7.dspace.org/server/api/eperson/groups/3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/subgroups"
      }
    },
    "page": {
      "number": 0,
      "size": 10,
      "totalPages": 1,
      "totalElements": 10
    }
}
```

### Changing subgroups
**PUT /api/eperson/groups/<:parent_group_uuid>/subgroups**

_Unsupported._ At this time, we do not support removing or replacing all subgroups in a single request. Please use `DELETE /api/core/groups/<:parent_group_uuid>/groups/<:sub_group_uuid>` to remove child groups one by one.

### Add a Group to a parent Group
**POST /api/eperson/groups/<:parentgroupuuid>/subgroups**

To add a child group to a parent group, perform a POST to the Sub groups of a Group endpoint when logged in as admin.

The actual sub group is part of the body using the uri-list

Example:
```bash
curl -i -X POST "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/subgroups"
  -H "Content-Type:text/uri-list"
  -d "https://api7.dspace.org/server/api/eperson/groups/05e3dbb8-332b-4487-a3f9-d78431b6cc02"
```

The group is mandatory

Return codes:
* 204: if the update succeeded 
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions 
* 404 Not found - if the parent group doesn't exist
* 422 Unprocessable Entity - if the specified group is not found, or if adding the group would create a cyclic reference (ie. A -> B -> C, -> mean contains we cannot add A to C)

### Remove a Group from a parent Group
**DELETE /api/eperson/groups/<:parent_group_uuid>/subgroups/<:sub_group_uuid>**

A DELETE request will result in removing a subgroup from the parent group

Example:
```bash
curl -i -X DELETE 
  "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/subgroups/05e3dbb8-332b-4487-a3f9-d78431b6cc02"
```

The above request would remove the mapping between the parent group with UUID `617cf46b-535c-42d5-9d22-327ce2eff6dc`
 and the child group with UUID `05e3dbb8-332b-4487-a3f9-d78431b6cc02`.

Return codes:
* 204: if the delete succeeded (including the case of no-op if the child group was not a subgroup) 
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the parent group doesn't exist
* 422 Unprocessable Entity - if the child group doesn't exist

## Epeople in a single EPerson Group

### Get Epeople for a single EPerson Group

**GET /api/eperson/groups/<:uuid>/epersons**

Retrieves direct child epeople of the given group.
Requesting indirect child epeople is not supported

Sample:
* GET /rest/api/eperson/groups/3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/epersons


```json
{
    "_embedded": {
      "epersons": [
        {
          "id": "a6086b34-3918-45b7-8ddd-9329a702a26a",
          "uuid": "a6086b34-3918-45b7-8ddd-9329a702a26a",
          "name": "atmirenv@gmail.com",
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
          "selfRegistered": false,
          "groups": null,
          "type": "eperson",
          "_links": {
            "self": {
              "href": "https://api7.dspace.org/server/api/eperson/epersons/a6086b34-3918-45b7-8ddd-9329a702a26a"
            }
          }
        }
      ]
    },
    "_links": {
      "self": {
        "href": "https://api7.dspace.org/server/api/eperson/groups/3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/epersons"
      }
    },
    "page": {
      "number": 0,
      "size": 10,
      "totalPages": 1,
      "totalElements": 10
    }
}
```

### Changing Epeople in a Group
**PUT /api/eperson/groups/<:groupuuid>/epersons**

_Unsupported._ At this time, we do not support removing or replacing all epersons in a single request. Please use `DELETE /api/eperson/groups/<:groupuuid>/epersons/<:epersonuuid>` to remove the epersons one by one.

### Add an EPerson to a parent Group
**POST /api/eperson/groups/<:groupuuid>/epersons**

To add an eperson to a parent group, perform a POST to the Epeople of a Group endpoint when logged in as admin.

The actual eperson is part of the body using the uri-list

Example:
```bash
curl -i -X POST "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/epersons"
  -H "Content-Type:text/uri-list"
  -d "https://api7.dspace.org/server/api/eperson/epersons/a6086b34-3918-45b7-8ddd-9329a702a26a"
```

The eperson is mandatory

Return codes:
* 204: if the update succeeded 
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions 
* 422: if the specified eperson is not found

### Remove an EPerson from a parent Group
**DELETE /api/eperson/groups/<:groupuuid>/epersons/<:epersonuuid>**

To remove an eperson from a parent group, perform a DELETE to the Epeople of a Group endpoint when logged in as admin.

Example:
```bash
curl -i -X DELETE 
  "https://api7.dspace.org/server/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/epersons/a6086b34-3918-45b7-8ddd-9329a702a26a"
```

The above request would remove the mapping between the group with UUID `617cf46b-535c-42d5-9d22-327ce2eff6dc`
 and the eperson with UUID `a6086b34-3918-45b7-8ddd-9329a702a26a`.


Return codes:
* 204: if the update succeeded (including the case of no-op if the eperson was not a member)
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions 
* 404 Not found - if the parent group doesn't exist
* 422 Unprocessable Entity - if the child group doesn't exist or if the specified eperson doesn't exist

## Search
**GET /api/eperson/groups/search/byMetadata?query=<:name>**

This supports a basic search in the metadata.
It will search in:
* UUID (exact match)
* group name

## Related DSpace Object of group
**GET /api/eperson/groups/<:uuid>/object** (READ-ONLY)

This returns the DSpace Object (Community, Collection) belonging to this Group.
This is only applicable for roles in that DSpace Object, e.g. the Community Administrator or Collection Submitter Group
