# Collections Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/collections**   

Provide access to the list of collections (DBMS based).

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/collections>

## Single Collection
**/api/core/collections/<:uuid>**

Provide detailed information about a specific community. The JSON response document is as follow
```json
{
  "uuid": "1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb",
  "name": "Collection of Sample Items",
  "handle": "10673/2",
  "metadata": [
    {
      "key": "dc.provenance",
      "value": "This field is for private provenance information. It is only visible to Administrative users and is not displayed in the user interface by default.",
      "language": null
    },
    {
      "key": "dc.rights.license",
      "value": "",
      "language": null
    },
    {
      "key": "dc.description",
      "value": "<p>This is a <em>DSpace Collection</em> which contains sample DSpace Items.</p>\r\n<p><strong>Collections in DSpace may only contain Items.</strong></p>\r\n<p>This particular Collection has its own logo (the <a href=\"http://www.opensource.org/\">Open Source Initiative</a> logo).</p>\r\n<p>This introductory text is editable by System Administrators, Community Administrators (of a parent Community) or Collection Administrators (of this Collection).</p>",
      "language": null
    },
    {
      "key": "dc.description.abstract",
      "value": "This collection contains sample items.",
      "language": null
    },
    {
      "key": "dc.description.tableofcontents",
      "value": "<p>This is the <strong>news</strong> section for this Collection. System Administrators, Community Administrators (of a parent Community) or Collection Administrators (of this Collection) can edit this News field.</p>",
      "language": null
    },
    {
      "key": "dc.rights",
      "value": "<p><em>If this collection had a specific copyright statement, it would be placed here.</em></p>",
      "language": null
    },
    {
      "key": "dc.title",
      "value": "Collection of Sample Items",
      "language": null
    }
  ],
  "type": "collection"
}
```

Exposed links:
* logo: link to the bitstream that represent the collection's logo
* license: link to the license template used by the collection
* defaultAccessConditions: link to the resource policies applied by default to new submissions in the collection

## Linked entities
### Logo
**/api/core/collections/<:uuid>/logo**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb/logo>

[It returns the bitstream representing the logo of this collection. See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

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
            "href": "https://dspace7.4science.it/dspace-spring-rest/api/authz/resourcePolicies/2844"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/5ad50035-ca22-4a4d-84ca-d5132f34f588/defaultAccessConditions"
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

## Creating a collection

**POST /api/core/collections?parentCommunity=<:communityUUID>**

To create a collection, perform as post with the JSON below when logged in as admin.

```
{
"name": "test collection",
"metadata": [
    {
        "key": "dc.title",
        "value": "test collection",
        "language": null
    }
    ]
}
```

## Updating a collection

**PUT /api/core/collections/<:uuid>**

Provide updated metadata information about a specific collection, when the update is completed the updated object will be returned. The JSON to update can be found below.
```
{
"uuid": "20263916-6a3d-4fdc-a44a-4616312f030c",
"name": "test collection",
"metadata": [
    {
        "key": "dc.title",
        "value": "test collection",
        "language": null
    },
    {
        "key": "dc.description",
        "value": "Test description",
        "language": null
    }
    ]
}
```  

## Deleting a collection

**DELETE /api/core/collections/<:uuid>**

Delete a collection.

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the community doesn't exist (or was already deleted)
