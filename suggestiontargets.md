# Suggestion Targets Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/suggestiontargets**   

_Unsupported._ The suggestion targets can be retrieved only by source or by target, see the search methods below. 

### single entry
**GET api/integration/suggestiontargets/<source:target-id>**

It returns the data from one target. This endpoint is accessible to the owner of the target profile and to the administrators

sample for a suggestion /api/integration/suggestiontargets/scopus:nhy567-9d6d-ty67-b905-fef0f8cae26
```json
{
    "id": "scopus:nhy567-9d6d-ty67-b905-fef0f8cae26",
    "display": "Digilio, Giuseppe",
    "source": "scopus",
    "total": 3,
    "type": "suggestiontarget",
    "_links": {
      "target": {
        "href": "https://dspace7.4science.cloud/server/api/core/items/nhy567-9d6d-ty67-b905-fef0f8cae26"
      },
      "suggestions": {
        "href": "https://dspace7.4science.cloud/server/api/integration/suggestions/search/findByTargetAndSource?target=nhy567-9d6d-ty67-b905-fef0f8cae26"
      },
      "self": {
        "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/scopus:nhy567-9d6d-ty67-b905-fef0f8cae26"
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
* suggestions: link to the suggestions entries

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
            "href": "https://dspace7.4science.cloud/server/api/core/items/gf3d657-9d6d-4a87-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestions/search/findByTargetAndSource?target=gf3d657-9d6d-4a87-b905-fef0f8cae26c&source=reciter"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/reciter:gf3d657-9d6d-4a87-b905-fef0f8cae26"
          }
        }
      },
      {
        "id": "reciter:nhy567-9d6d-ty67-b905-fef0f8cae26",
        "display": "Digilio, Giuseppe",
        "source": "reciter",
        "total": 12,
        "type": "suggestiontarget",
        "_links": {
          "target": {
            "href": "https://dspace7.4science.cloud/server/api/core/items/nhy567-9d6d-ty67-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestions/search/findByTargetAndSource?target=nhy567-9d6d-ty67-b905-fef0f8cae26&source=reciter"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/reciter:nhy567-9d6d-ty67-b905-fef0f8cae26"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/search/findBySource?source=reciter"
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
            "href": "https://dspace7.4science.cloud/server/api/core/items/gf3d657-9d6d-4a87-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestions/search/findByTargetAndSource?target=gf3d657-9d6d-4a87-b905-fef0f8cae26c&source=reciter"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/reciter:gf3d657-9d6d-4a87-b905-fef0f8cae26"
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
            "href": "https://dspace7.4science.cloud/server/api/core/items/gf3d657-9d6d-4a87-b905-fef0f8cae26"
          },
          "suggestions": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestions/search/findByTargetAndSource?target=gf3d657-9d6d-4a87-b905-fef0f8cae26&source=scopus"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/scopus:gf3d657-9d6d-4a87-b905-fef0f8cae26"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/suggestiontargets/search/findByTarget?target=gf3d657-9d6d-4a87-b905-fef0f8cae26"
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
