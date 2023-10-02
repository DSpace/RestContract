# Correction Type Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains the various types of corrections that authorized DSpace Users can suggest via the Quality Assurance Framework. The DSpace Users Corrections are indeed one of the possible [Quality Assurence Source](qualityassurancesources.md). 

## Main Endpoint
**/api/config/correctiontypes**

```json
{
  "_embedded": {
      "correctiontypes": [
        {
          "id" : "request-withdrawn",
          "topic" : "WITHDRAWN",
          "creationForm" : "provideReason",
          "type" : "correctiontype",
          "_links" : {
            "self" : {
              "href" : "http://localhost/api/config/correctiontypes/request-withdrawn"
            }
          }
        },
        {
          "id" : "request-reinstate",
          "topic" : "REINSTATE",
          "creationForm" : "provideReason",
          "type" : "correctiontype",
          "_links" : {
            "self" : {
              "href" : "http://localhost/api/config/correctiontypes/request-reinstate"
            }
          }
        }
    ]
  },
  "_links": {
    "self": {
      "href": "https://api7.dspace.org/server/api/config/correctiontypes"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
```
List all the available correction types in the system. The correction types are defined via the spring file `config/spring/api/correction-types.xml`

A sample can be found at https://demo.dspace.org/server/#/server/api/config/correctiontypes

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated

## Single Correction Type
**/api/config/correctiontypes/<:id>**

```json
{
  "id" : "request-withdrawn",
  "topic" : "/REQUEST/WITHDRAWN",
  "creationForm" : "requestWithdrawn",
  "type" : "correctiontype",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/correctiontypes/request-withdrawn"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if no correction type exists with such id

## Search methods
### findByItem
**/api/config/correctiontypes/search/findByItem?uuid=<:itemUUID>**

This search method will return only the correction types that can be used by the current user on the specified item. So it would provide a subset of the correction types filtering out both correction types that make no sense for the specified item than correction types that are not allowed to the current user.
Out of box a 

Parameters:
* The `uuid` uuid of the item

A sample search would be `/server/api/config/correctiontypes/search/findByItem?uuid=f1ba2a86-2f67-4e36-b572-c7956cb0fa32'

Assuming that the sample `config/spring/api/correction-types.xml` data model has been loaded, it would respond with

```json
{
  "_embedded" : {
    "correctiontypes" : [ 
      {
        "id" : "request-withdrawn",
        "topic" : "/REQUEST/WITHDRAWN",
        "creationForm" : "requestWithdrawn",
        "type" : "correctiontype",
        "_links" : {
          "self" : {
            "href" : "http://localhost/api/config/correctiontypes/request-withdrawn"
          }
        }
    ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/correctiontypes/search/findByItem?uuid=3825e2b4-8791-4bf6-b1ea-a56d74c5a54f"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid parameter is missing or is not an UUID
* 401 Unauthorized - if you are not authenticated or if there are insufficient permissions on provided item
* 422 Unprocessable Entity - if the Item of provided uuid not found