# Suggestion Targets Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/suggestiontargets**   

_Unsupported._ The suggestion targets can be retrieved only by target and/or by source, see the search methods below. 

### single entry
**GET api/integration/suggestiontargets/<source:target-id>**

It returns the data from one target. This endpoint is accessible to the owner of the target profile and to the administrators

sample for a suggestion /api/integration/suggestiontargets/openaire%3Abbb7feb2-7099-44cd-a896-1d47305a6a44

```json
{
  "id" : "openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44",
  "display" : "Andrea Bollini",
  "source" : "openaire",
  "total" : 10,
  "type" : "suggestiontarget",
  "_links" : {
    "target" : {
      "href" : "http://localhost:8080/server/api/integration/suggestiontargets/openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44/target"
    },
    "self" : {
      "href" : "http://localhost:8080/server/api/integration/suggestiontargets/openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44"
    }
  }
}
```

Attributes
* the *display* attribute is the preferred name of the linked Person
* the *source* attribute is the key that identifies the source of the suggestion
* the *total* attribbute is the number of related suggestions. Must be greater than 0

Exposed links:
* target: link to the items that represent the person to whom the suggestions are proposed

Status codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if there are no suggestions for the requested profile (when 403 doesn't apply) 


### Search methods
#### findBySource
**/api/integration/suggestiontargets/search/findBySource?source=<:source>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* source: mandatory, the key that identify the source to query

It returns the list of targets with their suggestion count for the specified source. Only targets that have at least one suggestion are returned. This endpoint is reserved to administrators

Sample of a resposne for /api/integration/suggestiontargets/search/findBySource?page=0&size=10&sort=display,ASC&source=openaire

```json
{
  "_embedded" : {
    "suggestiontargets" : [ {
      "id" : "openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44",
      "display" : "Andrea Bollini",
      "source" : "openaire",
      "total" : 10,
      "type" : "suggestiontarget",
      "_links" : {
        "target" : {
          "href" : "http://localhost:8080/server/api/integration/suggestiontargets/openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44/target"
        },
        "self" : {
          "href" : "http://localhost:8080/server/api/integration/suggestiontargets/openaire:bbb7feb2-7099-44cd-a896-1d47305a6a44"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/server/api/integration/suggestiontargets/search/findBySource?page=0&size=10&sort=display,ASC&source=openaire"
    }
  },
  "page" : {
    "size" : 10,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Status codes:
* 200 Ok - if the operation succeed
* 400 Bad Request - if the source parameter is missing
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in as an administrator

#### findByTarget
**/api/integration/suggestiontargets/search/findByTarget?target=<:target-uuid>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* target: mandatory, the uuid that identify the target profile

It returns the list of targets with their suggestion count for the specified profile. Only suggestion targets that have at least one suggestion are returned. This endpoint is reserved to administrators and the owner of the target profile

Example:
```json
{
  "_embedded": {
    "suggestiontargets": [
      {
        "id": "reciter:gf3d657-9d6d-4a87-b905-fef0f8cae26",
        "display": "Bollini, Andrea",
        "source": "reciter",
        "total": 31,
        "type": "suggestiontarget",
        "_links": {
          "target": {
            "href": "https://demo.dspace.org/server/api/core/items/gf3d657-9d6d-4a87-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://demo.dspace.org/server/api/integration/suggestions/search/findByTargetAndSource?target=gf3d657-9d6d-4a87-b905-fef0f8cae26c&source=reciter"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/suggestiontargets/reciter:gf3d657-9d6d-4a87-b905-fef0f8cae26"
          }
        }
      },
      {
        "id": "scopus:gf3d657-9d6d-4a87-b905-fef0f8cae26",
        "display": "Bollini, Andrea",
        "source": "scopus",
        "total": 11,
        "type": "suggestiontarget",
        "_links": {
          "target": {
            "href": "https://demo.dspace.org/server/api/core/items/gf3d657-9d6d-4a87-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://demo.dspace.org/server/api/integration/suggestions/search/findByTargetAndSource?target=gf3d657-9d6d-4a87-b905-fef0f8cae26&source=scopus"
          },
          "self": {
            "href": "https://demo.dspace.org/server/api/integration/suggestiontargets/scopus:gf3d657-9d6d-4a87-b905-fef0f8cae26"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://demo.dspace.org/server/api/integration/suggestiontargets/search/findByTarget?target=gf3d657-9d6d-4a87-b905-fef0f8cae26"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

Status codes:
* 200 Ok - if the operation succeed
* 204 No Content - if no suggestion targets are available for the specified profile
* 400 Bad Request - if the target parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
