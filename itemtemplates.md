# Collection Item Template Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/itemtemplates**   

_Unsupported._ No list of all item templates is supported

## Single Item template
**/api/core/itemtemplates/<:uuid>**

Provide detailed information about a specific item. The JSON response document is as follow
```json
{
  "uuid": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
  "name": null,
  "handle": null,
  "metadata": {
    "dc.type": [
      {
        "value": "Journal Article",
        "language": "en",
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ]
  },
  "inArchive": false,
  "discoverable": false,
  "withdrawn": false,
  "lastModified": "2019-06-24T00:40:54.970+0000",
  "type": "item"
}
```
 
## Creating an item template
**POST /api/core/collections/<:uuid>/itemtemplate**

See the [collection endpoint](collections.md#create-item-template) for details

## Updating item template metadata
**PATCH /api/core/itemtemplates/<:uuid>**

Item metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

No other properties can be modified

#### Delete Item template
**DELETE /api/core/itemtemplates/<:uuid>**

A collection's item template can be deleted, automatically ensuring the relationship between the collection and the item template is removed as well.

Curl example:
```
curl 'https://demo.dspace.org/server/api/core/itemtemplates/34d343c5-feb7-4a54-b07f-20a92debe061' \
 -XDELETE \
 -H 'Authorization: Bearer eyJhbGciOiJI...'
```

* The item template is determined using the ID in the URL

Status codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the item template doesn't exist
