# Bitstreams Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/bitstreams**   

Provide access to the bitstreams (DBMS based). It returns the list of existent bitstreams.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/bitstreams>

## Single Bitstream
**/api/core/bitstreams/<:uuid>**

Provide detailed information about a specific bitstream. The JSON response document is as follow
```json
{
  "uuid": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
  "name": null,
  "handle": null,
  "metadata": {},
  "bundleName": null,
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

## Patch operations

Bitstream metadata can be modified as described in [Modifying metadata via Patch](metadata-patch.md).

Additional bitstream properties can be modified via Patch as described below.

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation to set not yet initialized information must return an error. See [general errors on PATCH requests](patch.md)

To change the sequenceId:

`curl --data '[ { "op": "replace", "path": "/sequenceId", "value": 2}]' -H "Authorization: Bearer ..." -H "content-type: application/json" -X PATCH ${dspace7-url}/api/core/bitstreams/${uuid}`


For example, starting with the following bitstream data:
```json
{
  "name" : "test.zip",
  "bundleName" : "ORIGINAL",
  "sequenceId" : 5
}
```
the change sequenceId operation will result in:
```json
{
  "name" : "test.zip",
  "bundleName" : "ORIGINAL",
  "sequenceId" : 2
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the bitstream doesn't exist
* 422 Unprocessable Entity - if the sequenceId already exists for the item containing the bitstream

## Linked entities
### Format
**GET /api/core/bitstreams/<:uuid>/format**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/format>

It returns the format of the bitstream

**PUT /api/core/bitstreams/<:uuid>/format**

Update the bitstream format of the bitstream

Sample CURL command:
```
curl -i -X PUT 'https://dspace7-entities.atmire.com/rest/api/core/bitstreams/6ba01288-8a5a-4acf-96f1-fd0730424a1f/format' -H 'Authorization: Bearer eyJhbGciO…' -H "Content-Type:text/uri-list" --data 'https://dspace7-entities.atmire.com/rest/api/core/bitstreamformats/6'
```

The uri-list should always contain exactly 1 bitstream format. This bitstream format will be assigned to the bitstream

Error codes:
* 200 OK - if the operation succeeded
* 400 Bad Request - if the bitstream format doesn't exist, or if the amount of bitstream formats is not 1
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not logged in with sufficient permissions
* 404 Not found - if the bitstream doesn't exist

### Content
**/api/core/bitstreams/<:uuid>/content**

Example: <http://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/content>

It returns the actual content (bits) described by the bitstream

Response Headers:

* **ETag**: contains the md5 checksum as recorded in the bitstream record
* **Content-Type**: the mimetype as recorded in the bitstream format registry 
* **Content-Length**: the size in bytes of the content as recorded in the bitstream record

The supported **Request Headers** are:
* If-Modified-Since: not implemented yet. Support for cache control
* Range: not implemented yet. Provide support to partial content download
* If-None-Match: not implemented yet. Support for cache control

## DELETE Method
Delete a bitstream. Only works for normal bitstreams in an Item (bundle), to delete a community or collection logo it is needed to interact with the community or collection endpoints

* 204 No content - if the operation succeed
* 401 Forbidden - if you are not authenticated
* 403 Unauthorized - if you are not loggedin with sufficient permissions
* 404 Not found - if the bitstream doesn't exist (or was already deleted)
* 422 Unprocessable Entity - if the bitstream is a community or collection logo
