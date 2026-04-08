# EditItem Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Single EditItem
**/api/core/edititems/<:id>:<:MODE>**

Provide detailed information about a specific edititem. The JSON response document is as follow
```json
{
	"id":"7a356e11-f719-4dae-ae44-fa93f21ee6a0:FIRST",
	"lastModified":"2020-10-12T16:06:39.021+0000",
	"sections":{
		"titleAndIssuedDate":{
			"dc.title":[
				{
					"value":"Title item",
					"language":null,
					"authority":null,
					"confidence":-1,
					"place":0
				}
			],
			"dc.date.issued":[
				{
					"value":"2010-06-18",
					"language":null,
					"authority":null,
					"confidence":-1,
					"place":0
				}
			]
		}
	},
	"type":"edititem",
  "uniqueType": "core.edititem"
}
```
Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 if you are not logged in with sufficient permissions to view the edititem
* 404 Not found - if the edititem or MODE doesn't exist


## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

### Add
To add a new value to an **existent metadata**  and the metadata must be defined in the submissionDefinition of current MODE ,the client must send a JSON Patch ADD operation as follow

`curl -X PATCH '{dspace7-url}/api/core/edititems/<:id>:<:MODE>' -H "Authorization: Bearer ..." -H 'Content-Type: application/json' --data '[{"op":"add","path":"/sections/<:name-of-the-form>/<:metadata>/-","value":{"value":"...","language":"...","authority":"...","confidence":-1}}]'

### Remove
It is possible to remove a specific metadatavalue if the metadata is defined in the submissionDefinition of current MODE
`curl --data '[{ "op": "remove", "path": "/sections/traditionalpageone/dc.subject/0"}]' -X PATCH ${dspace7-url}/api/core/edititems/<:id>:<:MODE>`

## Find available modes
**/api/core/edititems/search/findModesById?uuid=<:id>**

Provide detailed information about edit item modes available to current user for Item having uuid passed as input parameter. 
The JSON response document is as follow
```json
{
  "_embedded": {
    "edititemmodes": [
      {
        "id": "FULL",
        "name": "FULL",
        "label": null,
        "submissionDefinition": "publication-edit",
        "type": "edititemmode",
        "uniqueType": "core.edititemmode",
        "_links": {
          "self": {
            "href": "https://{dspace-cris-backend-url}/server/api/core/edititemmodes/FULL"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://{dspace-cris-backend-url}/server/api/core/edititems/search/findModesById?uuid=9880d9e1-5441-4e14-a6e8-6cf453bc25f9"
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
Return codes:
* 200 OK - if the operation succeed
* 400 Bad request - if the id parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated

## Find edit items by submitter
**/api/core/edititems/search/findBySubmitter?uuid=<:submitter-uuid>**

Returns a paginated list of edit items submitted by the specified user (EPerson). Each EditItem is returned **without a specific mode**, using the mode identifier `"none"` in the composite ID. This means the returned EditItems have minimal information and no populated sections, as they are not bound to any particular edit mode configuration.

To work with a specific edit mode for an item, use the `/api/core/edititems/search/findModesById` endpoint to discover available modes, then access the item via `/api/core/edititems/{uuid}:{mode}`.

**Note:** This endpoint requires READ permission on the specified EPerson. It retrieves all archived items where the submitter field matches the provided UUID.

The JSON response document is as follows:
```json
{
  "_embedded": {
    "edititems": [
      {
        "id": "7a356e11-f719-4dae-ae44-fa93f21ee6a0:none",
        "lastModified": "2020-10-12T16:06:39.021+0000",
        "sections": {},
        "type": "edititem",
        "uniqueType": "core.edititem",
        "_links": {
          "self": {
            "href": "http://{dspace-url}/server/api/core/edititems/7a356e11-f719-4dae-ae44-fa93f21ee6a0:none"
          },
          "item": {
            "href": "http://{dspace-url}/server/api/core/edititems/7a356e11-f719-4dae-ae44-fa93f21ee6a0:none/item"
          },
          "collection": {
            "href": "http://{dspace-url}/server/api/core/edititems/7a356e11-f719-4dae-ae44-fa93f21ee6a0:none/collection"
          },
          "modes": {
            "href": "http://{dspace-url}/server/api/core/edititems/7a356e11-f719-4dae-ae44-fa93f21ee6a0:none/modes"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://{dspace-url}/server/api/core/edititems/search/findBySubmitter?uuid=a1b2c3d4-5678-90ab-cdef-1234567890ab"
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

Parameters:
* `uuid` (required): The UUID of the EPerson (submitter) whose edit items should be retrieved

Return codes:
* 200 OK - if the operation succeeds
* 400 Bad request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you don't have READ permission on the specified EPerson


