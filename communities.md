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
  "metadata": [
    {
      "key": "dc.description",
      "value": "",
      "language": null
    },
    {
      "key": "dc.description.abstract",
      "value": "This is a test community to hold content for the OR2017 demostration",
      "language": null
    },
    {
      "key": "dc.description.tableofcontents",
      "value": "",
      "language": null
    },
    {
      "key": "dc.rights",
      "value": "",
      "language": null
    },
    {
      "key": "dc.title",
      "value": "OR2017 - Demonstration",
      "language": null
    }
  ],
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
**/api/core/communities/<:uuid>/logo**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/communities/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/logo>

[It returns the bitstream representing the logo of this community. See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

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

```
{
    "name": "test creation",
    "metadata": [
        {
            "key": "dc.title",
            "value": "test creation",
            "language": null
        }
    ]
}
```

### Creating subCommunity

**POST /api/core/communities**

To create a sub level community, perform as post with the JSON below to the communities endpoint when logged in as admin.

```
{
    "name": "test subcommunity",
    "parentCommunities": [
    {
        "community": {URI: "/api/core/communities/b8872eba-1a79-4b8b-a8f6-55fa8f73197b"}
    }
    ],
    "metadata": [
        {
            "key": "dc.title",
            "value": "test subcommunity",
            "language": null
        }
    ]
}
```


## Updating a community

**PUT /api/core/communities/<:uuid>**

Provide updated metadata information about a specific community, when the update is completed the updated object will be returned. The JSON to update can be found below.
```
{
    "id": "b8872eba-1a79-4b8b-a8f6-55fa8f73197b",
    "uuid": "b8872eba-1a79-4b8b-a8f6-55fa8f73197b",
    "name": "test new title",
    "handle": "123456789/60631",
    "metadata": [
        {
            "key": "dc.title",
            "value": "test new title",
            "language": null
        },
        {
            "key": "dc.description",
            "value": "An example description",
            "language": "en"
        }
    ],
    "type": "community"
}
```  


## Deleting a community

**DELETE /api/core/communities/<:uuid>**

Delete a community.

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist (or was already deleted)
