# Communities Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/communities**   

Provide access to the communities (DBMS based). It returns the list of existent communities.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/communities>

## Single Community 
**/api/core/communities/<:uuid>**

Provide detailed information about a specific community. The JSON response document is as follow
```json
{
  "uuid": "7669c72a-3f2a-451f-a3b9-9210e7a4c02f",
  "name": "OR2017 - Demonstration",
  "handle": "10673/11",
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

### SubCommunities
**/api/core/communities/<:uuid>/subcommunities**

Example: t.b.p

It returns the sub-communities within this community

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
 

### Collections
**/api/core/communities/<:uuid>/collections**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/collections>

It returns the collections within this community

The supported parameters are:
* page, size [see pagination](README.md#Pagination)

### Logo
#### Retrieve Logo
**GET /api/core/communities/<:uuid>/logo**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/logo>

It returns the bitstream representing the logo of this community. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

#### Create Logo
**POST /api/core/communities/<:uuid>/logo**

To be used on a community without a logo

Curl example:
```
curl 'https://dspace7.4science.cloud/dspace-spring-rest/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
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
curl 'https://dspace7.4science.cloud/dspace-spring-rest/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
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
curl 'https://dspace7.4science.cloud/dspace-spring-rest/api/core/communities/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
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

### Search methods
#### top
**/api/core/communities/search/top**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/communities/search/top>

The supported parameters are:
* page, size [see pagination](README.md#Pagination)

It returns the top level communities in the repository (i.e. the communities that doesn't have a parent)

#### subCommunities
**/api/core/communities/search/subCommunities?parent=<:uuid>**

Example: not available

The supported parameters are:
* **(mandatory)** parent, the UUID of the parent community
* page, size [see pagination](README.md#Pagination)

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
  "id": "b8872eba-1a79-4b8b-a8f6-55fa8f73197b",
  "uuid": "b8872eba-1a79-4b8b-a8f6-55fa8f73197b",
  "name": "test new title",
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
