# Bitstreams Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/bitstreams**   

_Unsupported._ As bitstreams are used for more than just storing the files of items (thumbnails, scripts & processes file output, ...) a general endpoint to loop over these has little added value. 


## Single Bitstream
**/api/core/bitstreams/<:uuid>**

Provide detailed information about a specific bitstream. The JSON response document is as follow
```json
{
  "uuid": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
  "name": null,
  "handle": null,
  "metadata": {},
  "sizeBytes": 8528,
  "checkSum": {
    "checkSumAlgorithm": "MD5",
    "value": "9d8f0f9e369cf12159d47c146c499cf4"
  },
  "sequenceId": null,
  "type": "bitstream"
}
```

Exposed links:
* format: link to the bitstream format resource associated with the bitstream (Adobe PDF, MS Word, etc.)
* content: link to access the actual content of the bitstream
* bundle: link to the bundle, not embedded
* thumbnail: the thumbnail of the bitstream

## Patch operations

Bitstream metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

## Linked entities
### Format
**GET /api/core/bitstreams/<:uuid>/format**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/format>

It returns the format of the bitstream

**PUT /api/core/bitstreams/<:uuid>/format**

Update the bitstream format of the bitstream

Sample CURL command:
```
curl -i -X PUT 'https://demo.dspace.org/server/api/core/bitstreams/6ba01288-8a5a-4acf-96f1-fd0730424a1f/format' -H 'Authorization: Bearer eyJhbGciO…' -H "Content-Type:text/uri-list" --data 'https://demo.dspace.org/server/api/core/bitstreamformats/6'
```

The uri-list should always contain exactly 1 bitstream format. This bitstream format will be assigned to the bitstream

Error codes:
* 200 OK - if the operation succeeded
* 400 Bad Request - if the bitstream format doesn't exist, or if the amount of bitstream formats is not 1
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bitstream doesn't exist

Keep in mind that there's a change to dc.format in the API related to bitstream formats:
* setting the bitstream format to a known type removes dc.format in the bitstream metadata
* setting the bitstream format to unknown also removes dc.format in the bitstream metadata
* setting dc.format in the metadata will never implicitly change the bitstream format to unknown
* setting dc.format in the metadata will require a separate REST call, it's not part of the /format request
* setting dc.format will currently not be denied when the format is known, but it's not recommended to set it as such

### Bundle
**GET /api/core/bitstreams/<:uuid>/bundle**

Example: <https://demo.dspace.org/server/#https://demo.dspace.org/server/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/bundle>

It returns the bundle of the bitstream

**PUT /api/core/bitstreams/<:uuid>/bundle**

Move the bitstream to another bundle

Sample CURL command:
```
curl -i -X PUT 'https://demo.dspace.org/server/api/core/bitstreams/6ba01288-8a5a-4acf-96f1-fd0730424a1f/bundle' -H 'Authorization: Bearer eyJhbGciO…' -H "Content-Type:text/uri-list" --data 'https://demo.dspace.org/server/api/core/bundles/0b3c0ebf-83bc-4017-afa1-9df37a1a065c'
```

The uri-list should always contain exactly 1 bitstream format. This bitstream format will be assigned to the bitstream

Error codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bitstream doesn't exist
* 422 Unprocessable Entity - if the bundle doesn't exist, or if the amount of bundles is not 1

### Content
**/api/core/bitstreams/<:uuid>/content**

Example: <https://demo.dspace.org/server/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/content>

It returns the actual content (bits) described by the bitstream

Supported Request Parameter:
* **authentication-token**: optional parameter that allows downloads for restricted files, a token needs to be requested from the [following endpoint](authentication.md#Request-short-lived-token) 

Response Headers:

* **ETag**: contains the md5 checksum as recorded in the bitstream record
* **Content-Type**: the mimetype as recorded in the bitstream format registry 
* **Content-Length**: the size in bytes of the content as recorded in the bitstream record

The supported **Request Headers** are:
* If-Modified-Since: not implemented yet. Support for cache control
* Range: not implemented yet. Provide support to partial content download
* If-None-Match: not implemented yet. Support for cache control

### Main Thumbnail
**/api/core/bitstreams/<:uuid>/thumbnail**

It returns the bitstream which represents the thumbnail of the specified bitstream.  
At this time, thumbnails are only supported for bitstreams in the `ORIGINAL` bundle.

Status codes:
* 200 OK - returning the thumbnail
* 204 No Content - if there is no thumbnail for the specified bitstream
* 401 Unauthorized - if you are not authenticated and don't have permissions on the bitstream or the thumbnail's metadata
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bitstream doesn't exist

## Search methods
### byItemHandle
**/api/core/bitstreams/search/byItemHandle?handle=<:prefix>/<:suffix>&sequence=<:sequence_id>&filename=<:filename>**

It returns the bitstream matching the given parameters, or an empty response

The supported parameters are:
* `handle`: mandatory, the item's handle
* `sequence`: optional, the bitstream's sequence ID
* `filename`: optional, the bitstream's file name
* either `sequence` or `filename` should be present

Return codes:
* 200 OK - if the operation succeed. This will contain the bitstream
* 204 No Content - if there is no bitstream matching the given parameters
* 400 Bad Request - if neither `sequence` nor `filename` is present
* 401 Unauthorized - if you are not authenticated and the bitstream metadata is not public
* 403 Forbidden - if you are not logged in with sufficient permissions to see the bitstream metadata
* 422 Unprocessable Entity - if the provided uuid cannot be resolved to an item regardless to the item status

## DELETE Method

### Single Bitstream Delete
Delete a bitstream. Works for normal bitstreams in an Item (bundle), and a community or collection logo

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not loggedin with sufficient permissions
* 404 Not found - if the bitstream doesn't exist (or was already deleted)
* 422 Unprocessable Entity - if the bitstream is a community or collection logo

### Multiple Bitstreams Delete
Delete a list of Bitstreams in one request. This will work for any list of Bitstreams that are attached to an item, will mostly be used in the item admin edit

* 204 No content - if the operation succeeded
* 401 Unauthorized -  if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if any of the bitstreams in the list haven't been found or are already deleted
* 422 Unprocessable Entity - If one or more of the given Bitstreams aren't attached to an Item.

A sample CURL command would be:
```
curl -i -X PATCH 'https://demo7.dspace.org/server/api/core/bitstreams -H 'Authorization: Bearer eyJhbGciO…' -H "content-type: application/json" --data '[ { "op": "remove", "path": "/bitstreams/12623672-25a9-4df2-ab36-699c4c240c7e"}, { "op": "remove", "path": "/bitstreams/5a3f7c7a-d3df-419c-8a2-f00ede62c60a"}]'
```

The Patch contents is:
```json
[
  {
    "op": "remove",
    "path": "/bitstreams/12623672-25a9-4df2-ab36-699c4c240c7e"
  },
  {
    "op": "remove",
    "path": "/bitstreams/5a3f7c7a-d3df-419c-8a2-f00ede62c60a"
  }
]
```
