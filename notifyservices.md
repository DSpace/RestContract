# Notify Services Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/core/notifyservices**

Provide access to the notifyservices (DBMS based). List all the notify services

A sample can be found at https://api7.dspace.org/server/#/server/api/core/notifyservices

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated

## Single Notify Service
**/api/core/notifyservices/<:id>**

```json
{
  "id" : 1,
  "name" : "service name",
  "description" : "service description",
  "url" : "service url",
  "ldnUrl" : "service ldn url",
  "notifyServiceInboundPatterns" :
  [
    {
    "pattern" : "patternA",
    "constraint" : "itemFilterA",
    "automatic" : false
    },
    {
      "pattern" : "patternB",
      "constraint" : "itemFilterB",
      "automatic" : true
    }
  ],
  "notifyServiceOutboundPatterns" : [ {
    "pattern" : "patternC",
    "constraint" : "itemFilterC"
  } ],
  "type" : "notifyservice",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/core/notifyservices/1"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if no notify service exists with such id

## Creating a new notify service
**POST /api/core/notifyservices**

Authenticated users can create a notify service. The content-type is JSON. An example JSON can be seen below:

```json
{
  "name": "service name",
  "description": "service description",
  "url": "service url",
  "ldnUrl": "service ldn url"
}
```

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if no notify service exists with such id
* 422 Unprocessable Entity - if Error parsing request body

## PATCH
### To record or update a notify service inbound pattern
PATCH /api/core/notifyservices/<:id>

This method allow users to create or update a notify service inbound pattern.
The PATCH body must follow the JSON PATCH specification

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns",
    "value": "{\"pattern\":\"patternA\",\"constraint\":\"itemFilterA\",\"automatic\":\"false\"}"
  }
]
```

### To record or update a notify service outbound pattern
PATCH /api/core/notifyservices/<:id>

This method allow users to create or update a notify service outbound pattern.
The PATCH body must follow the JSON PATCH specification

```json
[
  {
    "op": "replace",
    "path": "notifyservices_outbound_patterns",
    "value": "{\"pattern\":\"patternA\",\"constraint\":\"itemFilterA\"}"
  }
]
```

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if the Notify Service doesn't exist (or was already deleted)

## Deleting a notify service

**DELETE /api/core/notifyservices/<:id>**

Delete a notify service.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if the notify service doesn't exist (or was already deleted)

## Search methods
### findByLdnUrl
**/api/core/notifyservices/search/byLdnUrl?ldnUrl=<:ldnUrl>**

Parameters:
* The `ldnUrl` ldnUrl of the notify service

A sample search would be `/server/api/core/notifyservices/search/byLdnUrl?ldnUrl=service_ldn_url'

```json
{
  "_embedded" : {
    "notifyservices" : [ {
      "id" : 1,
      "name" : "service name one",
      "description" : "service description one",
      "url" : "service url one",
      "ldnUrl" : "service_ldn_url",
      "notifyServiceInboundPatterns" :
      [
        {
          "pattern" : "patternA",
          "constraint" : "itemFilterA",
          "automatic" : false
        }
      ],
      "notifyServiceOutboundPatterns" : [ {
        "pattern" : "patternB",
        "constraint" : "itemFilterB"
      } ],
      "type" : "notifyservice",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/core/notifyservices/1"
        }
      }
    }, {
      "id" : 2,
      "name" : "service name two",
      "description" : "service description two",
      "url" : "service url two",
      "ldnUrl" : "service_ldn_url",
      "notifyServiceInboundPatterns" : [ ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "notifyservice",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/core/notifyservices/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/core/notifyservices/search/byLdnUrl?ldnUrl=service_ldn_url"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 2,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the ldnUrl parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated

### findByPattern
**/api/core/notifyservices/search/byPattern?pattern=<:pattern>**

Parameters:
* The `pattern` pattern of the notify service inbound

A sample search would be `/server/api/core/notifyservices/search/byPattern?pattern=patternA'

```json
{
  "_embedded" : {
    "notifyservices" : [ {
      "id" : 3,
      "name" : "service name one",
      "description" : "service description one",
      "url" : "service url one",
      "ldnUrl" : "service ldn url one",
      "notifyServiceInboundPatterns" : [ {
        "pattern" : "patternA",
        "constraint" : "itemFilterA",
        "automatic" : false
      } ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "notifyservice",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/core/notifyservices/3"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/core/notifyservices/search/byPattern?pattern=patternA"
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
* 400 Bad Request - if the pattern parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated

