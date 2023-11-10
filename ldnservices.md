# LDN Notify Services Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/ldn/ldnservices**

Provide access to the notifyservices (DBMS based). List all the LDN notify services

A sample can be found at https://api7.dspace.org/server/#/server/api/ldn/ldnservices

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users

## Single LDN Notify Service
**/api/ldn/ldnservices/<:id>**

```json
{
  "id" : 1,
  "name" : "service name",
  "description" : "service description",
  "url" : "service url",
  "ldnUrl" : "service ldn url",
  "enabled" : true,
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
      "href" : "http://localhost/api/ldn/ldnservices/1"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users
* 404 Not found - if no LDN notify service exists with such id

## Creating a new LDN notify service
**POST /api/ldn/ldnservices**

Only administrator users can create LDN notify service. The content-type is JSON. An example JSON can be seen below:

```json
{
  "name": "service name",
  "description": "service description",
  "url": "service url",
  "ldnUrl": "service ldn url",
  "enabled" : true,
  "notifyServiceInboundPatterns":
  [
    {"pattern":"patternA","constraint":"itemFilterA","automatic":true},
    {"pattern":"patternB","constraint":null,"automatic":false}
  ],
  "notifyServiceOutboundPatterns":
  [
    {"pattern":"patternC","constraint":"itemFilterC"}
  ]
}
```

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators
* 404 Not found - if no LDN notify service exists with such id
* 422 Unprocessable Entity - if Error parsing request body

## PATCH

### Add
PATCH /api/ldn/ldnservices/<:id>

to add name to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/name",
    "value": "service name"
  }
]
```

to add description to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/description",
    "value": "service description"
  }
]
```

to add ldnUrl to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/ldnurl",
    "value": "service ldnUrl"
  }
]
```

to add url to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/url",
    "value": "url value"
  }
]
```

to add inboundPatterns to ldn notify service

-adding object to the last, this array isn't sorted so, not supported adding to index like "notifyServiceInboundPatterns[index]"

```json
[
  {
    "op": "add",
    "path": "notifyServiceInboundPatterns/-",
    "value": {"pattern":"patternA","constraint":"itemFilterA","automatic":"false"}
  }
]
```

to add outboundPatterns to ldn notify service

-adding object to the last, this array isn't sorted so, not supported adding to index like "notifyServiceOutboundPatterns[index]"

```json
[
  {
    "op": "add",
    "path": "notifyServiceOutboundPatterns/-",
    "value": {"pattern":"patternA","constraint":"itemFilterA"}
  }
]
```

to add pattern to specific inboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyServiceInboundPatterns[0]/pattern",
    "value": "patternA"
  }
]
```

to add constraint to specific inboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyServiceInboundPatterns[0]/constraint",
    "value": "itemFilterA"
  }
]
```


to add pattern to specific outboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyServiceOutboundPatterns[0]/pattern",
    "value": "patternA"
  }
]
```

to add constraint to specific outboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyServiceOutboundPatterns[0]/constraint",
    "value": "itemFilterA"
  }
]
```

### Replace

to update the name of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/name",
    "value": "service name"
  }
]
```

to update the description of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/description",
    "value": "service description"
  }
]
```

to update the ldnUrl of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/ldnurl",
    "value": "service ldnUrl"
  }
]
```

to update the url of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/url",
    "value": "url value"
  }
]
```

to update enabled of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/enabled",
    "value": false
  }
]
```

to replace all inboundPatterns of ldn notify service,
if value contains an empty array all inboundPatterns will be removed

```json
[
  {
    "op": "replace",
    "path": "notifyServiceInboundPatterns",
    "value": [{"pattern":"patternC","constraint":"itemFilterC","automatic":"false"}, {"pattern":"patternD","constraint":"itemFilterD","automatic":"false"}]
  }
]
```

to replace inboundPattern of ldn notify service at specific index

```json
[
  {
    "op": "replace",
    "path": "notifyServiceInboundPatterns[0]",
    "value": {"pattern":"patternD","constraint":"itemFilterD","automatic":"true"}
  }
]
```

to replace all outboundPatterns of ldn notify service,
if value contains an empty array all outboundPatterns will be removed

```json
[
  {
    "op": "replace",
    "path": "notifyServiceOutboundPatterns",
    "value": [{"pattern":"patternC","constraint":"itemFilterC"}, {"pattern":"patternD","constraint":"itemFilterD"}]
  }
]
```

to replace outboundPattern of ldn notify service at specific index

```json
[
  {
    "op": "replace",
    "path": "notifyServiceOutboundPatterns[0]",
    "value": {"pattern":"patternD","constraint":"itemFilterD"}
  }
]
```

to replace pattern of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyServiceInboundPatterns[1]/pattern",
    "value": "patternB"
  }
]
```

to replace constraint of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyServiceInboundPatterns[1]/constraint",
    "value": "itemFilterB"
  }
]
```

to replace automatic of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyServiceInboundPatterns[1]/automatic",
    "value": "true"
  }
]
```

to replace pattern of specific outboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyServiceOutboundPatterns[1]/pattern",
    "value": "patternB"
  }
]
```

to replace constraint of specific outboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyServiceOutboundPatterns[0]/constraint",
    "value": "itemFilterA"
  }
]
```

### Remove

to remove name or ldnUrl from ldn notify service

```json
[
  {
    "op": "remove",
    "path": "/name"
  }
]
```

```json
[
  {
    "op": "remove",
    "path": "/ldnurl"
  }
]
```

status codes:
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators
* 404 Not found - if the LDN Notify Service doesn't exist (or was already deleted)
* 422 Unprocessable Entity - name and ldnUrl fields are mandatory and can't be removed


to remove description value from ldn notify service

```json
[
  {
    "op": "remove",
    "path": "/description"
  }
]
```

to remove url value from ldn notify service

```json
[
  {
    "op": "remove",
    "path": "/url"
  }
]
```

to remove all inboundPatterns of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyServiceInboundPatterns"
  }
]
```

to remove inboundPattern from ldn notify service at specific index

```json
[
  {
    "op": "remove",
    "path": "notifyServiceInboundPatterns[0]"
  }
]
```

to remove all outboundPatterns of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyServiceOutboundPatterns"
  }
]
```

to remove outboundPattern from ldn notify service at specific index

```json
[
  {
    "op": "remove",
    "path": "notifyServiceOutboundPatterns[1]"
  }
]
```

to remove constraint from specific inboundPattern of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyServiceInboundPatterns[0]/constraint"
  }
]
```

to remove constraint from specific outboundPattern of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyServiceOutboundPatterns[0]/constraint"
  }
]
```

Patch Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators
* 404 Not found - if the LDN Notify Service doesn't exist (or was already deleted)

## Deleting LDN notify service

**DELETE /api/ldn/ldnservices/<:id>**

Delete LDN notify service.

* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators
* 404 Not found - if the LDN notify service doesn't exist (or was already deleted)

## Search methods
### findByLdnUrl
**/api/ldn/ldnservices/search/byLdnUrl?ldnUrl=<:ldnUrl>**

Parameters:
* The `ldnUrl` ldnUrl of the LDN notify service

A sample search would be `/server/api/ldn/ldnservices/search/byLdnUrl?ldnUrl=service_ldn_url'

```json
{
  "id" : 1,
  "name" : "service name one",
  "description" : "service description one",
  "url" : "service url one",
  "ldnUrl" : "service_ldn_url",
  "enabled" : true,
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
      "href" : "http://localhost/api/ldn/ldnservices/1"
    }
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the ldnUrl parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users

### findByInboundPattern
**/api/ldn/ldnservices/search/byInboundPattern?pattern=<:pattern>**

Parameters:
* The `pattern` the inbound pattern of the LDN notify service

A sample search would be `/server/api/ldn/ldnservices/search/byInboundPattern?pattern=request-review'

```json
{
  "_embedded" : {
    "ldnservices" : [ {
      "id" : 1,
      "name" : "service name one",
      "description" : "service description one",
      "url" : "https://service.ldn.org/about",
      "ldnUrl" : "https://service.ldn.org/inbox",
      "enabled" : false,
      "notifyServiceInboundPatterns" : [ {
        "pattern" : "request-review",
        "constraint" : "itemFilterA",
        "automatic" : false
      } ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "ldnservice",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/ldn/ldnservices/1"
        }
      }
    }, {
      "id" : 2,
      "name" : "service name two",
      "description" : "service description two",
      "url" : "https://service2.ldn.org/about",
      "ldnUrl" : "https://service2.ldn.org/inbox",
      "enabled" : false,
      "notifyServiceInboundPatterns" : [ {
        "pattern" : "request-review",
        "constraint" : "itemFilterA",
        "automatic" : false
      } ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "ldnservice",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/ldn/ldnservices/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/ldn/ldnservices/search/byInboundPattern?pattern=request-review"
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
* 400 Bad Request - if the pattern parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users