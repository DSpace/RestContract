# Collections Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/collections**   

Provide access to the list of collections (DBMS based).

Example: <https://demo.dspace.org/server/#/server/api/core/collections>

## Single Collection
**/api/core/collections/<:uuid>**

Provide detailed information about a specific collection. The JSON response document is as follow
```json
{
  "uuid": "1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb",
  "name": "Collection of Sample Items",
  "handle": "10673/2",
  "archivedItemsCount": 6,
  "metadata": {
    "dc.description": [
      {
        "value": "<p>This is a <em>DSpace Collection</em> which contains sample DSpace Items.</p>\r\n<p><strong>Collections in DSpace may only contain Items.</strong></p>\r\n<p>This particular Collection has its own logo (the <a href=\"http://www.opensource.org/\">Open Source Initiative</a> logo).</p>\r\n<p>This introductory text is editable by System Administrators, Community Administrators (of a parent Community) or Collection Administrators (of this Collection).</p>",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description.abstract": [
      {
        "value": "This collection contains sample items.",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description.tableofcontents": [
      {
        "value": "<p>This is the <strong>news</strong> section for this Collection. System Administrators, Community Administrators (of a parent Community) or Collection Administrators (of this Collection) can edit this News field.</p>",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.provenance": [
      {
        "value": "This field is for private provenance information. It is only visible to Administrative users and is not displayed in the user interface by default.",
        "language": null,
        "authority": null,
        "confidence": -1
      },
      {
        "value": "Second provenance value",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.rights": [
      {
        "value": "<p><em>If this collection had a specific copyright statement, it would be placed here.</em></p>",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.rights.license": [
      {
        "value": "",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Collection of Sample Items",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "type": "collection"
}
```

Exposed links:
* logo: link to the bitstream that represent the collection's logo
* license: link to the license template used by the collection
* defaultAccessConditions: link to the resource policies applied by default to new submissions in the collection
* parentCommunity: the community containing this collection
* adminGroup: the Collection Administrator group
* submittersGroup: the Collection Submitters group
* itemReadGroup: the Collection Default item READ rights group
* bitstreamReadGroup: the Collection Default bitstream READ rights group
* workflowGroups/<:workflow-role>: the Collection Workflow groups

Other properties:
* archivedItemsCount - The count of the items in the given container. It returns -1 when counting items feature 
is disabled at backend.

### Search methods
#### findSubmitAuthorized
**/api/core/collections/search/findSubmitAuthorized**

The supported parameters are:
* `query`: limit the returned collections to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the collection name
* `page`, `size` [see pagination](README.md#Pagination)

It returns the list of collections where the current user is authorized to submit

Return codes:
* 200 OK - if the operation succeeds

#### findSubmitAuthorizedByEntityType
**/api/core/collections/search/findSubmitAuthorizedByEntityType?query=<:query>&entityType=<:entityTypeLabel>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* entityType: mandatory, the label of the entity type  field the collection must have

It returns the list of collections where the current user is authorized to submit and deal with the request entity type

eg:
/api/core/collections/search/findSubmitAuthorizedByEntityType?entityType=<:entityType>
/api/core/collections/search/findSubmitAuthorizedByEntityType?entityType=Publication

retrieve all the collections that deal with the entity type 'Publication'  where the current user is authorized to submit

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the entityType parameter is missing or invalid


#### findSubmitAuthorizedByCommunity
**/api/core/collections/search/findSubmitAuthorizedByCommunity?uuid=<:uuid>**

The supported parameters are:
* `query`: limit the returned collections to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the collection name
* `page`, `size` [see pagination](README.md#Pagination)
* `uuid`: mandatory, the uuid of the community

It returns the list of collections which are direct children of the specified community where the current user is authorized to submit

Return codes:
* 200 OK - if the operation succeeds
* 400 Bad Request - if the uuid parameter is missing or invalid

#### findSubmitAuthorizedByCommunityAndEntityType
**/api/core/collections/search/findSubmitAuthorizedByCommunityAndEntityType?uuid=<:uuid>&query=<:query>&entityType=<:entityTypeLabel>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the community
* entityType: mandatory, the label of the entity type  field the collection must have

It returns the list of collections where the current user is authorized to submit and deal with the request entity type

eg:
/api/core/collections/search/findSubmitAuthorizedByCommunityAndEntityType?uuid=<:uuid>&entityType=<:entityType>
/api/core/collections/search/findSubmitAuthorizedByCommunityAndEntityType?uuid=<:uuid>&entityType=Publication

retrieve all children collections of the community that deal with the entity type 'Publication' where the current user is authorized to submit

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid or entityType parameters are missing or invalid

#### findAdminAuthorized
**/api/core/collections/search/findAdminAuthorized**

Get the list of all collections the current user is admin for.

The supported parameters are:
* `query`: limit the returned collections to those with metadata values matching the query terms.
  The query is also used to build a prefix query. It can be used to implement
  an autosuggest feature over the collection name
* `page`, `size` [see pagination](README.md#Pagination)

Return codes:
* 200 OK - if the operation succeeds
* 401 Unauthorized - if you are not authenticated

## Patch operations

Collection metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

## Linked entities
### Logo
#### Retrieve Logo
**GET /api/core/collections/<:uuid>/logo**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo>

It returns the bitstream representing the logo of this collection. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

#### Create Logo
**POST /api/core/collections/<:uuid>/logo**

To be used on a collection without a logo

Curl example:
```
curl 'https://demo.dspace.org/server/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo' \
 -XPOST -H 'Content-Type: multipart/form-data' \
 -H 'Authorization: Bearer eyJhbGciOiJI...' \
 -F "file=@Downloads/test.png"
```

* The collection is determined using the ID in the URL
* The file is uploaded using multipart/form-data

It returns the created bitstream. [See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

The REST API can support Content-Length and Content-MD5 headers to verify integrity

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the collection doesn't exist
* 412 Precondition Failed - if there is a discrepancy between the declared size or checksum and the computed one
* 422 Unprocessable Entity - if there was no file, or if the collection already contains a logo

This endpoint only accepts one file at a time. If multiple files are uploaded, any extra files will be ignored.

#### Replace Logo

Replacing a logo will require deleting the logo and creating a new logo hereafter

#### Delete Logo
**DELETE /api/core/bitstreams/<:uuid>**

Use the [bitstream delete endpoint](bitstreams.md#delete-method) for removing the collection logo

If the bitstream is delete, this automatically ensures the relationship between the collection and the logo is removed as well.

### License
**/api/core/collections/<:uuid>/license**

Return information about the license template in use by the collection. The json representation is as follow
```json
{
  "custom": false,
  "text": "NOTE: PLACE YOUR OWN LICENSE HERE\nThis sample license is provided for informational purposes only.\n\nNON-EXCLUSIVE DISTRIBUTION LICENSE\n\nBy signing and submitting this license, you (the author(s) or copyright\nowner) grants to DSpace University (DSU) the non-exclusive right to reproduce,\ntranslate (as defined below), and/or distribute your submission...."
}
```

* custom (**READ-ONLY**): can be true or false. True means that a custom license has been set for the collection otherwise the site license template is used and returned in the text attribute
* text: contains the textual value of the license template to use for submission in the collection

### Item template
#### Retrieve Item template
**GET /api/core/collections/<:uuid>/itemtemplate**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/itemtemplate>

It returns the item representing the item template of this collection. [See the item endpoint for more info](items.md#Single Item)

#### Create Item template
**POST /api/core/collections/<:uuid>/itemtemplate**

To be used on a collection without a item template.
The metadata is included in JSON

```json
{
  "metadata": {
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "inArchive": false,
  "discoverable": false,
  "withdrawn": false,
  "type": "item"
}
```

* The collection is determined using the ID in the URL
* The metadata is uploaded using JSON
* The properties inArchive, discoverable, withdrawn can be omitted or false, but not true

It returns the created item. [See the item endpoint for more info](items.md#Single Item)

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the collection doesn't exist
* 422 Unprocessable Entity - if inArchive, discoverable, withdrawn was set to true, or if the collection already contains a itemtemplate

#### Replace Item template
**PATCH /api/core/itemtemplates/<:uuid>**

See the [item template endpoint](itemtemplates.md#updating-item-template-metadata) for details

#### Delete Item template
**DELETE /api/core/itemtemplates/<:uuid>**

See the [item template endpoint](itemtemplates.md#delete-item-template) for details

### Default Access Conditions
**/api/core/collections/<:uuid>/defaultAccessConditions**

It returns the resource policies applied by default to new submissions in the collection. They are the DEFAULT\_BITSTREAM\_READ policies of the collection

The json representation is as follow
```json
{
  "_embedded": {
    "resourcePolicies": [
      {
        "id": 2844,
        "name": null,
        "groupUUID": "11cc35e5-a11d-4b64-b5b9-0052a5d15509",
        "action": "DEFAULT_BITSTREAM_READ",
        "type": "resourcePolicy",
        "_links": {
          "self": {
            "href": "https://demo.dspace.org/server/api/authz/resourcePolicies/2844"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/core/collections/5ad50035-ca22-4a4d-84ca-d5132f34f588/defaultAccessConditions"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```
see also the [ResourcePolicies endpoint](resourcepolicies.md)

### Mapped items
**/api/core/collections/<:uuid>/mappedItems**

This is a Read-only endpoint to retrieve the mapped items for this collection.

Item mappings can only be modified via [/items/[uuid]/mappedCollections](items.md#mapped-collections)

The request will return a list of items 

### Collection Harvesting Settings
**GET /api/core/collections/<:uuid>/harvester**

It returns the harvesting settings for the current collection. This information is only accessible for users with collection administration permissions

The harvest_type can be any of:
* NONE
* METADATA_ONLY
* METADATA_AND_REF
* METADATA_AND_BITSTREAMS

The harvest_status can be any of:
* READY
* BUSY
* QUEUED
* OAI_ERROR
* UNKNOWN_ERROR

The metadata_config_id can be one of the ids from the [Harvester Metadata Endpoint](harvestermetadata.md)

A sample json response:

```json
{
  "harvest_type": "METADATA_ONLY",
  "oai_source": "https://dspace.mit.edu/oai/request",
  "oai_set_id": "col_1721.1_114174",
  "harvest_message": null,
  "metadata_config_id": "dc",
  "harvest_status": "READY",
  "harvest_start_time": null,
  "last_harvested": null,
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/core/collections/6f944500-c300-449a-9023-a5ad8bd21160/harvester"
    }
  },
  "_embedded": {
    "metadata_configs": {
      "configs": [
        {
           "id": "dc",
           "label": "Simple Dublin Core",
           "nameSpace": "http://www.openarchives.org/OAI/2.0/oai_dc/"
        },
        {
           "id": "qdc",
           "label": "Qualified Dublin Core",
           "nameSpace": "http://purl.org/dc/terms/"
        },
        {
           "id": "dim",
           "label": "DSpace Intermediate Metadata",
           "nameSpace": "http://www.dspace.org/xmlns/dspace/dim"
        }
      ],
      "_links": {
        "self": {
          "href": "https://demo.dspace.org/server/api/config/harvestermetadata"
        }
      }
    }
    
  }
}
```

A sample json response if no harvesting is enabled:

```json
{
  "harvest_type": "NONE",
  "oai_source": null,
  "oai_set_id": null,
  "harvest_message": null,
  "metadata_config_id": null,
  "harvest_status": null,
  "harvest_start_time": null,
  "last_harvested": null,
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/core/collections/6f944500-c300-449a-9023-a5ad8bd21160/harvester"
    }
  },
  "_embedded": {
    "harvestermetadata": {
      "configs": [
        {
           "id": "dc",
           "label": "Simple Dublin Core",
           "nameSpace": "http://www.openarchives.org/OAI/2.0/oai_dc/"
        },
        {
           "id": "qdc",
           "label": "Qualified Dublin Core",
           "nameSpace": "http://purl.org/dc/terms/"
        },
        {
           "id": "dim",
           "label": "DSpace Intermediate Metadata",
           "nameSpace": "http://www.dspace.org/xmlns/dspace/dim"
        }
      ],
      "_links": {
        "self": {
          "href": "https://demo.dspace.org/server/api/config/harvestermetadata"
        }
      }
    }
    
  }
}
```

### Changing Collection Harvesting Settings
**PUT /api/core/collections/<:uuid>/harvester**

It updates the harvesting settings for the current collection. This information can only be updated by users with collection administration permissions

A sample json request:

```json
{
  "harvest_type": "METADATA_ONLY",
  "oai_source": "https://dspace.mit.edu/oai/request",
  "oai_set_id": "col_1721.1_114174",
  "metadata_config_id": "dc"
}
```

A sample json request to disable harvesting is:

```json
{
  "harvest_type": "NONE"
}
```

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the collection doesn't exist
* 422 Unprocessable Entity - if the harvest_type or the metadata_config_id is not valid

### Parent Community
**/api/core/collections/<:uuid>/parentCommunity**

It returns the community containing this collection, e.g. for trail purposes.  
If a collection is part of multiple parent communities, it only returns one community.

Return codes:
* 200 OK - if the parent community exists and returned
* 204 No content - if the collection exists but the parent community doesn't exist
* 401 Unauthorized - if you are not authenticated and the current collection or parent community is not public
* 403 Forbidden - if you are not logged in with sufficient permissions to retrieve the current collection or parent community
* 404 Not found - if the current collection doesn't exist

### Groups

This includes group management for the type of groups:
* adminGroup: the Collection Administrator group
* submittersGroup: the Collection Submitters group
* itemReadGroup: the Collection Default item READ rights group
* bitstreamReadGroup: the Collection Default bitstream READ rights group
* workflowGroups/<:workflow-role>: the Collection Workflow groups

#### Collection administrators
**/api/core/collections/<:uuid>/adminGroup**

Endpoints for managing the collection administrators

##### Retrieve collection administrators
**GET /api/core/collections/<:uuid>/adminGroup**

Example: /server/api/core/collections/7669c72a-3f2a-451f-a3b9-9210e7a4c02f/adminGroup

It returns the EPerson Group representing the administrators of this collection. [See the EPerson Group endpoint for more info](epersongroups.md#single-eperson-group)

Return codes:
* 200 OK - if the admin group exists and is returned
* 204 No content - if the current collection exists but the admin group doesn't exist
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions to retrieve the groups, parent community admins and collection admins can retrieve the group
* 404 Not found - if the current collection doesn't exist

##### Create collection administrators group
**POST /api/core/collections/<:uuid>/adminGroup**

To be used on a collection without collection administrators

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
* 404 Not found - if the collection doesn't exist
* 422 Unprocessable Entity - if the name was included, if permanent was set to true, or if the collection already contains an administrator group

##### Modifying the collection administrators group

All modifications to the group will be performed directly on the Group endpoint:
* Adding members will need to use the [EPerson Group endpoint](epersongroups.md#add-an-eperson-to-a-parent-group)
* Adding sub-groups will need to use the [EPerson Group endpoint](epersongroups.md#add-a-group-to-a-parent-group)

Modifying the collection administrators group will be authorized for admins, parent community admins and collection admins

##### Delete the collection administrators group
**DELETE /api/core/collections/<:uuid>/adminGroup**

To be used on a collection with an administrator group

Status codes:
* 204 No content - if the delete succeeded (including the case of no-op if the collection didn't contain an administrator group) 
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only admins, parent community admins and collection admins can delete the group
* 404 Not found - if the collection doesn't exist
* 422: if the collection didn't contain an administrator group

#### Collection Submitters
**/api/core/collections/<:uuid>/submittersGroup**

Endpoints for managing the Collection Submitters group

This works identical to the [Collection administrators](#collection-administrators),
except the collection administrators can also create the submitters group.

#### Collection Default item READ rights group
**/api/core/collections/<:uuid>/itemReadGroup**

Endpoints for managing the Collection Default item READ rights group

This works identical to the [Collection administrators](#collection-administrators),
except the collection administrators can also create the Collection Default item READ rights group.

#### Collection Default bitstream READ rights group
**/api/core/collections/<:uuid>/bitstreamReadGroup**

Endpoints for managing the Collection Default bitstream READ rights group

This works identical to the [Collection administrators](#collection-administrators),
except the collection administrators can also create the Collection Default bitstream READ rights group

#### Collection Workflow groups
**/api/core/collections/<:uuid>/workflowGroups/<:workflow-role>**

Endpoints for managing the Collection Workflow groups

This works similar to the [Collection administrators](#collection-administrators).  
The differences are:
* The collection administrators can also create the Collection Workflow groups
* The <:workflow-role> can be any role configured in the workflow with scope Collection. A HAL link for each of these groups will be included

The workflow role can be e.g.:
* reviewer
* editor
* finaleditor
* reviewmanagers

##### Delete a collection workflow group
**DELETE /api/core/collections/<:uuid>/workflowGroups/<:workflow-role>**

Delete the Group associated with a Workflow role.

Error messages:
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 Unprocessable entity - if the role still has pending workflow tasks (deleting the group in that case may cause tasks to end up in an invalid state)

## Creating a collection

**POST /api/core/collections?parent=<:communityUUID>**

To create a collection, perform as post with the JSON below when logged in as admin.

```json
{
  "name": "test collection",
  "metadata": {
    "dc.title": [
      {
        "value": "test collection",
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
 * 422 UNPROCESSABLE ENTITY - if the parent community doesn't exist (the REST URI /api/core/collections still exists)

## Updating a collection

**PUT /api/core/collections/<:uuid>**

Provide updated metadata information about a specific collection, when the update is completed the updated object will be returned. The JSON to update can be found below.

```json
{
  "uuid": "20263916-6a3d-4fdc-a44a-4616312f030c",
  "handle": "10673/2",
  "metadata": {
    "dc.title": [
      {
        "value": "test collection",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.description": [
      {
        "value": "Test description",
        "language": null,
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "type": "collection"
}
```  

Error messages:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the collection doesn't exist
* 422 UNPROCESSABLE ENTITY - Altering one of the non-editable parameters will result in a 422 UNPROCESSABLE ENTITY error. The non-editable parameters are optional, but if they are specified, they have to remain identical to the current value. The id, uuid, handle and type are non-editable.

## Deleting a collection

**DELETE /api/core/collections/<:uuid>**

Delete a collection.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the collection doesn't exist (or was already deleted)
