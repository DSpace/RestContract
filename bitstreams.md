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
  "metadata": [],
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

## Linked entities
### Format
**/api/core/bitstreams/<:uudi>/format**

Example: <http://dspace7.4science.it/dspace-spring-rest/#http://dspace7.4science.it/dspace-spring-rest/api/core/bitstreams/8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2/format>

It returns the format of the bitstream

### Content
**/api/core/bitstreams/<:uudi>/content**

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