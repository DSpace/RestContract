# Communities Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/communities**   

Provide access to the communities (DBMS based). It returns the list of existent communities.

Example: <https://demo.dspace.org/server/#/server/api/core/communities>

## Single Community 
**/api/core/communities/<:uuid>**

Provide detailed information about a specific community. The JSON response document is as follow
```json
{
  "uuid": "7669c72a-3f2a-451f-a3b9-9210e7a4c02f",
  "name": "OR2017 - Demonstration",
  "handle": "10673/11",
  "archivedItemsCount": 12,
  "metadata": {
    "dc.description": [
      {
        "value": "First description",
        "language": null,
        "authority": null,
        "confidence": -1
      },
      {
        "value": "Second description",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description.abstract": [
      {
        "value": "This is a test community to hold content for the OR2017 demostration",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description.tableofcontents": [
      {
        "value": "",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.rights": [
      {
        "value": "",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "OR2017 - Demonstration",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "type": "community"
}
```

Exposed links:
* subcommunities: list of sub-communities within this community
* collections: list of collections within this community
* logo: link to the bitstream that represent the community's logo
* parentCommunity: the community containing this community
* adminGroup: the Community Administrator group

Other properties:
* archivedItemsCount - The count of the items in the given container. It returns -1 when counting items feature
  is disabled at backend.

## Linked entities

### SubCommunities
**/api/core/communities/<:uuid>/subcommunities**

Example: t.b.p

It returns the sub-communities within this community

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
 

### Collections
**/api/core/communities/<:uuid>/collections**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/collections>

It returns the collections within this community

The supported parameters are:
* page, size [see pagination](README.md#Pagination)

### Logo
#### Retrieve Logo
**GET /api/core/communities/<:uuid>/logo**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/logo>

It returns the bitstream representing the logo of this community. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

#### Create Logo
**POST /api/core/communities/<:uuid>/logo**

To be used on a community without a logo

Curl example:
```
curl 'https://demo.dspace.org/server/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
 -XPOST -H 'Content-Type: multipart/form-data' \
 -H 'Authorization: Bearer eyJhbGciOiJI...' \
 -F "file=@Downloads/test.png"
```

* The community is determined using the ID in the URL
* The file is uploaded using multipart/form-data

It returns the created bitstream. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

The REST API can support Content-Length and Content-MD5 headers to verify integrity

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist
* 412 Precondition Failed - if there is a discrepancy between the declared size or checksum and the computed one
* 422 Unprocessable Entity - if there was no file, or if the community already contains a logo

This endpoint only accepts one file at a time. If multiple files are uploaded, any extra files will be ignored.

#### Replace Logo
**PUT /api/core/communities/<:uuid>/logo**

To be used on a community with a logo

Curl example:
```
curl 'https://demo.dspace.org/server/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
 -XPUT -H 'Content-Type: multipart/form-data' \
 -H 'Authorization: Bearer eyJhbGciOiJI...' \
 -F "file=@Downloads/test.png"
```

* The community is determined using the ID in the URL
* The file is uploaded using multipart/form-data

It returns the created bitstream. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

The REST API can support Content-Length and Content-MD5 headers to verify integrity

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist
* 412 Precondition Failed - if there is a discrepancy between the declared size or checksum and the computed one
* 422 Unprocessable Entity - if there was no file, or if the community didn't contain a logo

This endpoint only accepts one file at a time. If multiple files are uploaded, any extra files will be ignored.

#### Delete Logo
**DELETE /api/core/communities/<:uuid>/logo**

To be used on a community with a logo

Curl example:
```
curl 'https://demo.dspace.org/server/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
 -XDELETE \
 -H 'Authorization: Bearer eyJhbGciOiJI...'
```

* The community is determined using the ID in the URL

Status codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist
* 422: if the community didn't contain a logo

### Parent Community
**/api/core/communities/<:uuid>/parentCommunity**

It returns the community containing this communities, e.g. for trail purposes.  
If a community is part of multiple parent communities, it only returns one community.

Return codes:
* 200 OK - if the parent community exists and returned
* 204 No content - if the current community exists but the parent community doesn't exist
* 401 Unauthorized - if you are not authenticated and the current community or parent community is not public
* 403 Forbidden - if you are not logged in with sufficient permissions to retrieve the current community or parent community
* 404 Not found - if the current community doesn't exist

### Groups
#### Community administrators
**/api/core/communities/<:uuid>/adminGroup**

Endpoints for managing the Community administrators

##### Retrieve Community administrators
**GET /api/core/communities/<:uuid>/adminGroup**

Example: /server/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/adminGroup

It returns the EPerson Group representing the administrators of this community. [See the EPerson Group endpoint for more info](epersongroups.md#single-eperson-group)

Return codes:
* 200 OK - if the admin group exists and is returned
* 204 No content - if the current community exists but the admin group doesn't exist
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to retrieve the groups. Only admins and (parent) community admins can retrieve the group
* 404 Not found - if the current community doesn't exist

##### Create Community administrators group
**POST /api/core/communities/<:uuid>/adminGroup**

To be used on a community without Community administrators

Perform a post with the JSON below.

```json
{
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
Contrary to the [EPerson Group endpoint](epersongroups.md#create-new-eperson-group), the name cannot be set here

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only admins and parent community admins can retrieve the group
* 404 Not found - if the community doesn't exist
* 422 Unprocessable Entity - if the name was included, if permanent was set to true, or if the community already contains an administrator group

##### Modifying the Community administrators group

All modifications to the group will be performed directly on the Group endpoint:
* Adding members will need to use the [EPerson Group endpoint](epersongroups.md#add-an-eperson-to-a-parent-group)
* Adding sub-groups will need to use the [EPerson Group endpoint](epersongroups.md#add-a-group-to-a-parent-group)

Modifying the Community administrators group will be authorized for admins and (parent) community admins

##### Delete the Community administrators group
**DELETE /api/core/communities/<:uuid>/adminGroup**

To be used on a community with an administrator group

Status codes:
* 204 No content - if the delete succeeded (including the case of no-op if the community didn't contain an administrator group) 
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only admins and (parent) community admins can delete the group
* 404 Not found - if the community doesn't exist

### Search methods
#### top
**/api/core/communities/search/top**

The supported parameters are:
* `page`, `size` [see pagination](README.md#Pagination)

It returns the top level communities in the repository (i.e. the communities that doesn't have a parent)

#### findAdminAuthorized
**/api/core/communities/search/findAdminAuthorized**

Get the list of all communities the current user is admin for.

The supported parameters are:
* `query`: limit the returned communities to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the community name
* `page`, `size` [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated

#### findEditAuthorized
**/api/core/communities/search/findEditAuthorized**

Get the list of all communities the current user is authorized to edit.

The supported parameters are:
* `query`: limit the returned communities to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the community name
* `page`, `size` [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated

#### findAddAuthorized
**/api/core/communities/search/findAddAuthorized**

Get the list of all communities the current user is authorized to add collections or communities to.

The supported parameters are:
* `query`: limit the returned communities to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the community name
* `page`, `size` [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated

## Creating communities

### Creating top level community

**POST /api/core/communities**

To create a top level community, perform as post with the JSON below to the communities endpoint when logged in as admin.

```json
{
  "name": "test creation",
  "metadata": {
    "dc.title": [
      {
        "value": "test creation",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ]
  }
}
```

### Creating subCommunity

**POST /api/core/communities?parent=<:communityUUID>**

To create a sub level community, perform as post with the JSON below to the communities endpoint when logged in as admin.

```json
{
  "name": "test subcommunity",
  "metadata": {
    "dc.title": [
      {
        "value": "test subcommunity",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ]
  }
}
```

Error messages:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 UNPROCESSABLE ENTITY - if the parent community doesn't exist (the REST URI /api/core/communities still exists)

## Updating a community

**PUT /api/core/communities/<:uuid>**

Provide updated metadata information about a specific community, when the update is completed the updated object will be returned. The JSON to update can be found below.

```json
{
  "uuid": "b8872eba-1a79-4b8b-a8f6-55fa8f73197b",
  "handle": "123456789/60631",
  "metadata": {
    "dc.title": [
      {
        "value": "test new title",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description": [
      {
        "value": "An example description",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "type": "community"
}
```  

Error messages:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist
* 422 UNPROCESSABLE ENTITY - Altering one of the non-editable parameters will result in a 422 UNPROCESSABLE ENTITY error. The non-editable parameters are optional, but if they are specified, they have to remain identical to the current value. The id, uuid, handle and type are non-editable.

## Deleting a community

**DELETE /api/core/communities/<:uuid>**

Delete a community.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist (or was already deleted)

## Patch operations

Community metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).
