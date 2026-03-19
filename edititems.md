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
**/api/core/edititems/search/search/findModesById?uuid=<:id>**

Provide detailed information about edit item modes available to current user for Item having uuid passed as input paraameter. 
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

