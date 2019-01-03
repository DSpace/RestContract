# Items Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/items**   

Provide access to the items (DBMS based). It returns the list of existent items.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/items>

## Single Item
**/api/core/items/<:uuid>**

Provide detailed information about a specific item. The JSON response document is as follow
```json
{
  "uuid": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
  "name": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
  "handle": "10673/20",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Stvilia, Besiki",
        "language": "en",
        "authority": null,
        "confidence": -1
      },
      {
        "value": "Lee, Dong Joon",
        "language": "en",
        "authority": null,
        "confidence": -1
      },
    ],
    "dc.identifier.url": [
      {
        "value": "http://europepmc.org/abstract/MED/28301533",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2017-06-24T00:40:54.970+0000",
  "type": "item"
}
```

Exposed links:
* bitstreams: list of bitstreams within the item
* owningCollection: the collection where the item belong to
* mappedCollections: the collections where the item is mapped to
* templateItemOf: the collection that have the item as template
 
## Creating an archived item

**POST /api/core/items?owningCollection:<:uuid>**

Administrators can directly create an archived item (bypassing the workflow). An example JSON can be seen below:

```
{
  "name": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Stvilia, Besiki",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Practices of research data curation in institutional repositories: A qualitative view from repository staff",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "type": "item"
}
```

## Updating item metadata

**PUT /api/core/items/<:uuid>**

Provide updated metadata information for an item, when the update is completed the updated object will be returned. The JSON to update can be found below.

```
{
  "id": "a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9",
  "uuid": "a8ba963f-d9c9-4198-b5a4-3f74e2ab6fb9",
  "name": "Test new title",
  "handle": "123456789/60636",
  "metadata": {
    "dc.contributor.author": [
      {
        "value": "Velasco, Mercedes",
        "language": "en",
        "authority": null,
        "confidence": -1
      }
    ],
    "dc.title": [
      {
        "value": "Test new title",
        "language": "pt_BR",
        "authority": null,
        "confidence": -1
      }
    ]
  },
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "type": "item"
}
```
 
## Patch operations

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To withdraw an item, `curl --data '{[ { "op": "replace", "path": "/withdrawn", "value": true}]}' -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The withdraw operation also requires an Authorization header.

For example, starting with the following item data:
```json
 "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the withdraw operation will result in:
```json
  "inArchive": false,
  "discoverable": true,
  "withdrawn": true,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```

To reinstate an item, `curl --data '{[ { "op": "replace", "path": "/withdrawn", "value": false}]}' -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The reinstate operation also requires an Authorization header.

For example, starting with the following item data:
```json
 "inArchive": false,
  "discoverable": true,
  "withdrawn": true,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the reinstate operation will result in:
```json
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```

To make an item private (or discoverable), `curl --data '[ { "op": "replace", "path": "/discoverable", "value": false}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/core/items/${item-uuid}`.  The discoverable operation also requires an Authorization header.


For example, starting with the following item data:
```json
  "inArchive": true,
  "discoverable": true,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
the discoverable operation will result in:
```json
  "inArchive": true,
  "discoverable": false,
  "withdrawn": false,
  "lastModified": "2018-05-17T16:53:15.250+0000",
  "type": "item"
```
## Linked entities
### Bitstreams
**/api/core/items/<:uuid>/bitstreams**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/bitstreams>

It returns the bitstreams within this item. See the bitstream endpoint for more info](bitstreams.md#Single Bitstream)

The supported parameters are:
* page, size [see pagination](README.md#Pagination)

### Owning Collection
**/api/core/items/<:uuid>/owningCollection**

Example: <https://dspace7.4science.it/dspace-spring-rest/#https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection>

It returns the collection where the item belong to

**PUT /api/core/items/<:uuid>/owningCollection**

The actual collection is part of the body using the uri-list
Example:

```curl -i -X PUT "https://dspace7.4science.it/dspace-spring-rest/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb/owningCollection" -H "Content-Type:text/uri-list" -d "https://dspace7.4science.it/dspace-spring-rest/api/core/collections/8e0928a0-047a-4369-8883-12669f32dd64"```

It updates the owning collection (moves the item)

Status codes:
* 204 No content - if the operation succeeded
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist
* 422 Unprocessable Entity - if the collection doesn't exist or the data cannot be resolved to a collection

### Mapped Collections
**GET /api/core/items/<:uuid>/mappedCollections**

Example:
```json
    "mappedCollections":
    [
      {
        "id": "16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9",
        "uuid": "16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9",
        "name": "Bachelor theses",
        "handle": "123456789/5287",
        "metadata": [
          {
            "key": "dc.description.abstract",
            "value": "",
            "language": null
          },
          {
            "key": "dc.title",
            "value": "Bachelor theses",
            "language": null
          }
        ],
        "type": "collection",
        "_links": {
          "license": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9/license"
          },
          "defaultAccessConditions": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9/defaultAccessConditions"
          },
          "logo": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9/logo"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9"
          }
        },
        "_embedded": {
          "logo": null,
          "defaultAccessConditions": {
            "_embedded": {
              "defaultAccessConditions": [
                {
                  "id": 28054,
                  "name": null,
                  "groupUUID": "9e1794b8-45e0-4869-9c2c-36c4b25ce856",
                  "action": "DEFAULT_BITSTREAM_READ",
                  "type": "resourcePolicy",
                  "_links": {
                    "self": {
                      "href": "https://dspace7-internal.atmire.com/rest/api/authz/resourcePolicies/28054"
                    }
                  }
                }
              ]
            },
            "_links": {
              "self": {
                "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/16a4b65b-3b3f-4ef5-8058-ef6f5a653ef9/defaultAccessConditions"
              }
            },
            "page": {
              "number": 0,
              "size": 1,
              "totalPages": 1,
              "totalElements": 1
            }
          }
        }
      },
      {
        "id": "320c0492-de1d-4646-9e69-193d36b366e9",
        "uuid": "320c0492-de1d-4646-9e69-193d36b366e9",
        "name": "Biology",
        "handle": "123456789/5285",
        "metadata": [
          {
            "key": "dc.description.abstract",
            "value": "",
            "language": null
          },
          {
            "key": "dc.title",
            "value": "Biology",
            "language": null
          }
        ],
        "type": "collection",
        "_links": {
          "license": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9/license"
          },
          "exportToZip": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9/exportToZip"
          },
          "defaultAccessConditions": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9/defaultAccessConditions"
          },
          "logo": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9/logo"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9"
          }
        },
        "_embedded": {
          "logo": null,
          "defaultAccessConditions": {
            "_embedded": {
              "defaultAccessConditions": [
                {
                  "id": 28050,
                  "name": null,
                  "groupUUID": "9e1794b8-45e0-4869-9c2c-36c4b25ce856",
                  "action": "DEFAULT_BITSTREAM_READ",
                  "type": "resourcePolicy",
                  "_links": {
                    "self": {
                      "href": "https://dspace7-internal.atmire.com/rest/api/authz/resourcePolicies/28050"
                    }
                  }
                }
              ]
            },
            "_links": {
              "self": {
                "href": "https://dspace7-internal.atmire.com/rest/api/core/collections/320c0492-de1d-4646-9e69-193d36b366e9/defaultAccessConditions"
              }
            },
            "page": {
              "number": 0,
              "size": 1,
              "totalPages": 1,
              "totalElements": 1
            }
          }
        }
      }
    ]
  }
```

It returns all the mapped collections the item is included in

On the item page, it should be referenced similar to:
```json
    "mappedCollections": {
      "href": "https://dspace7-internal.atmire.com/rest/api/core/items/95e5d7d9-ef4e-4e35-86cc-07bfe2f0e355/mappedCollections"
    }
```

**POST /api/core/items/<item:uuid>/mappedCollections?collection=<collection:uuid>**

A POST request will result in creating a new mapping between the item and collection
If the collection exists and is neither the owning nor mapped collection for the item, the relation should be created

**DELETE /api/core/items/<item:uuid>/mappedCollections?collection=<collection:uuid>**

A DELETE request will result in removing an existing mapping between the item and collection
If the collection exists and is a mapped collection for the item, the relation should be deleted

### Template Item
**/api/core/items/<:uuid>/templateItemOf**

Example: to be provided

It returns the collection that have the item as template

## Deleting a collection

**DELETE /api/core/items/<:uuid>**

Delete an item.

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the item doesn't exist (or was already deleted)
