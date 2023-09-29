# Bundles Endpoints

[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint

**/api/core/bundles**

_Unsupported._ The main endpoint is not relevant and should not be implemented

## Single Bundle

**/api/core/bundles/<:uuid>**

Provide detailed information about a specific bundle. A sample JSON response document is included below:

```json
{
  "uuid": "d3599177-0408-403b-9f8d-d300edd79edb",
  "name": "ORIGINAL",
  "handle": null,
  "metadata": {
    "dc.title": [
      {
        "value": "ORIGINAL",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ]
  },
  "type": "bundle",
  "_links": {
    "primarybitstream": {
      "href": "https://api7.dspace.org/server/api/core/bitstreams/ac49f361-4ffd-47a4-8eb2-e6c73c3f3e76"
    },
    "bitstreams": {
      "href": "https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams"
    },
    "item": {
      "href": "https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/item"
    },
    "self": {
      "href": "https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb"
    }
  },
  "_embedded": {
    "bitstreams": [
      {
        "id": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
        "uuid": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
        "name": "atmire-logo.jpeg",
        "handle": null,
        "metadata": {
          "dc.description": [
            {
              "value": "",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ],
          "dc.source": [
            {
              "value": "atmire-logo.jpeg",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ],
          "dc.title": [
            {
              "value": "atmire-logo.jpeg",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ]
        },
        "sizeBytes": 7568,
        "checkSum": {
          "checkSumAlgorithm": "MD5",
          "value": "59a4cfd819ce67ba7252efc52d1eb99b"
        },
        "sequenceId": null,
        "type": "bitstream",
        "_links": {
          "content": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/1ce6db0e-662f-4a13-ba87-c371ad664b14/content"
          },
          "format": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/1ce6db0e-662f-4a13-ba87-c371ad664b14/format"
          },
          "self": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/1ce6db0e-662f-4a13-ba87-c371ad664b14"
          }
        }
      },
      {
        "id": "4dd9621f-a464-4192-bc17-d70f68845bdc",
        "uuid": "4dd9621f-a464-4192-bc17-d70f68845bdc",
        "name": "atmire-logo.jpeg.jpg",
        "handle": null,
        "metadata": {
          "dc.description": [
            {
              "value": "Generated Thumbnail",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ],
          "dc.source": [
            {
              "value": "Written by FormatFilter org.dspace.app.mediafilter.JPEGFilter on 2018-05-30T14:28:19Z (GMT).",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ],
          "dc.title": [
            {
              "value": "atmire-logo.jpeg.jpg",
              "language": null,
              "authority": null,
              "confidence": -1,
              "place": 0
            }
          ]
        },
        "sizeBytes": 4825,
        "checkSum": {
          "checkSumAlgorithm": "MD5",
          "value": "78647c1fab73a7aa7c97fdfc538ee6d4"
        },
        "sequenceId": null,
        "type": "bitstream",
        "_links": {
          "content": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/4dd9621f-a464-4192-bc17-d70f68845bdc/content"
          },
          "format": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/4dd9621f-a464-4192-bc17-d70f68845bdc/format"
          },
          "self": {
            "href": "https://api7.dspace.org/server/api/core/bitstreams/4dd9621f-a464-4192-bc17-d70f68845bdc"
          }
        }
      }
    ]
  }
}
```

Exposed links:

* primarybitstream: link to the primary bitstream if present, it will be embedded
* bitstreams: link to the list of bitstreams displayed according to the order specified by the bundle, it will be
  embedded
* item: link to the item

## Creating a bundle

Bundles are only created as part of an item, the contract can be found at the [item endpoint](items.md#bundles)

## Deleting a bundle

**DELETE /api/core/bundles/<:uuid>**

Deleting a bundle will delete all bitstreams in the bundle

## Bitstreams

**GET /api/core/bundles/<:uuid>/bitstreams**

Example: <https://api7.dspace.org/server/#https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams>

It returns the bitstreams within this bundle. See the [bitstream endpoint](bitstreams.md#Single-Bitstream) for more info

The supported parameters are:

* page, size [see pagination](README.md#Pagination)

**POST /api/core/bundles/<:uuid>/bitstreams**

TODO: the item has to be known as well when creating a new bitstream.
See https://jira.duraspace.org/browse/DS-4317?focusedCommentId=63099&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-63099

Example: <https://api7.dspace.org/server/#https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams>

Curl example:

```bash
curl 'https://api7.dspace.org/server/api/core/bundles/d3599177-0408-403b-9f8d-d300edd79edb/bitstreams' \
 -XPOST -H 'Content-Type: multipart/form-data' \
 -H 'Authorization: Bearer eyJhbGciOiJI...' \
 -F "file=@Downloads/test.html" \
 -F 'properties={ "name": "test.html", "metadata": { "dc.description": [ { "value": "example file", "language": null, "authority": null, "confidence": -1, "place": 0 } ]}, "bundleName": "ORIGINAL" };type=application/json'
```

* The item is determined using the ID in the URL
* The file is uploaded using multipart/form-data
* The metadata of the bitstream is included as a json property, and with layout, it looks like:

```json
{
  "name": "test.html",
  "metadata": {
    "dc.description": [
      {
        "value": "example file",
        "language": null,
        "authority": null,
        "confidence": -1,
        "place": 0
      }
    ]
  }
}
```

The bitstream properties can contain:

* The filename to be stored (optional)
* metadata for the bitstream (optional)

It returns the created bitstream. See the bitstream endpoint for more [info](bitstreams.md#Single Bitstream)

The REST API can support Content-Length and Content-MD5 headers to verify integrity

Status codes:

* 201 Created - if the operation succeed
* 400 Bad Request - if there was no file
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bundle doesn't exist
* 412 Precondition Failed - if there is a discrepancy between the declared size or checksum and the computed one

This endpoint only accepts one file at a time. If multiple files are uploaded, any extra files will be ignored.

## Ordering bitstreams

**PATCH /api/core/bundles/<:uuid>**

Bitstreams may be re-ordered with the PATCH `move` operation.

Current list of bitstreams:

```json
{
  "bitstreams": [
    {
      "id": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
      "uuid": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
      "_links": {
        "self": {
          "href": "https://api7.dspace.org/server/api/core/bitstreams/1ce6db0e-662f-4a13-ba87-c371ad664b14"
        }
      }
    },
    {
      "id": "4dd9621f-a464-4192-bc17-d70f68845bdc",
      "uuid": "4dd9621f-a464-4192-bc17-d70f68845bdc",
      "_links": {
        "self": {
          "href": "https://api7.dspace.org/server/api/core/bitstreams/4dd9621f-a464-4192-bc17-d70f68845bdc"
        }
      }
    }
  ]
}
```

Example request, moving the last bitstream to the first position:

```json
[
  {
    "op": "move",
    "from": "/_links/bitstreams/1/href",
    "path": "/_links/bitstreams/0/href"
  }
]
```

New list of bitstreams:

```json
{
  "bitstreams": [
    {
      "id": "4dd9621f-a464-4192-bc17-d70f68845bdc",
      "uuid": "4dd9621f-a464-4192-bc17-d70f68845bdc",
      "_links": {
        "self": {
          "href": "https://api7.dspace.org/server/api/core/bitstreams/4dd9621f-a464-4192-bc17-d70f68845bdc"
        }
      }
    },
    {
      "id": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
      "uuid": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
      "_links": {
        "self": {
          "href": "https://api7.dspace.org/server/api/core/bitstreams/1ce6db0e-662f-4a13-ba87-c371ad664b14"
        }
      }
    }
  ]
}
```

## Primary bitstream

The primary bitstream is exposed as a link, and can be modified

**/api/core/bundles/<:uuid>/primaryBitstream**

### Retrieve Primary bitstream

The Primary bitstream can be retrieved using a GET, this may result in `null`

**GET /api/core/bundles/<:uuid>/primaryBitstream**

### Create Primary bitstream

If the Primary bitstream doesn't exist, it can be created using a POST

**POST /api/core/bundles/<:uuid>/primaryBitstream**

This should use `Content-Type:text/uri-list` with a single URI value which represents the new Bitstream to mark as
primary.

```bash
curl -i -X POST "https://demo7.dspace.org/api/core/bundles/[uuid]/primaryBitstream" -H "Content-type:text/uri-list" -d "https://demo7.dspace.org/api/core/bitstreams/[uuid]"
```

Status codes:

* 201 Created - if the operation succeeded
* 400 Bad Request - if there already was a primary bitstream
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bundle doesn't exist
* 422 Unprocessable Entity - if the bitstream does not exist or is not part of this bundle

### Update Primary bitstream

If the Primary bitstream already exists, it can be changed using a PUT

**PUT /api/core/bundles/<:uuid>/primaryBitstream**

This should use `Content-Type:text/uri-list` with a single URI value which represents the new Bitstream to mark as
primary.

```bash
curl -i -X PUT "https://demo7.dspace.org/api/core/bundles/[uuid]/primaryBitstream" -H "Content-type:text/uri-list" -d "https://demo7.dspace.org/api/core/bitstreams/[uuid]"
```

Status codes:

* 200 OK - if the operation succeeded
* 400 Bad Request - if there was no primary bitstream
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bundle doesn't exist
* 422 Unprocessable Entity - if the bitstream does not exist or is not part of this bundle

### Removing Primary bitstream

If the Primary bitstream already exists, and no primary bitstream should be used anymore, it can be changed using a
DELETE. This will **not** delete the bitstream nor unassign it from the bundle, it will just ensure the bundle has no
primary bitstream anymore

**DELETE /api/core/bundles/<:uuid>/primaryBitstream**

This should not have any content

Status codes:

* 204 No Content - if the operation succeeded
* 400 Bad Request - if there was no primary bitstream
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the bundle doesn't exist
