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
  "url" : "https://service-name.org/about",
  "ldnUrl" : "https://service-name.org/ldn-inbox",
  "enabled" : true,
  "score" : "0.375",
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

  Attributes
* id: numerical identificator assigned by the rest to the service itself
* name: a short name associated to the service
* description: a description for the service, i.e. the reason for the service
* url: the URL for users to check out more information about the service
* ldnUrl: the URL of the LDN Inbox
* enabled: This proeprty is a simple boolean, it defines if the service is selected as one of the active ones
* score: This property defines to the safety score assigned by the user
* inboundPatterns: an array that contains inbound pattern defined by 3 properties:
    * pattern: the defining pattern name for the pattern itself
    * constraint: a filter put in place during the pattern creation
    * automatic: The automatic property is a boolean property that defines the behaviour during an item archiving operation;
      * automatic:TRUE = all the patterns flagged with the true value for the automatic property, for which a positive response is returned, a  notification on the corresponding service
      * automatic:FALSE = all the patterns flagged with the false value for the automatic property, needs to be explicitly declared from the user during a submission in the related submission section COAR Notify

* outboundPatterns: an array that contains inbound pattern defined by 3 properties:
    * pattern: the defining pattern name for the pattern itself
    * constraint: a filter put in place during the pattern creation

## Creating a new LDN notify service
**POST /api/ldn/ldnservices**

Only administrator users can create LDN notify service. The content-type is JSON. An example JSON can be seen below:

```json
{
  "name": "service name",
  "description": "service description",
  "url": "https://service-name.org/about",
  "ldnUrl": "https://service-name.org/ldn-inbox",
  "score" : "0.765",
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
    "value": "https://service-name.org/ldn-inbox"
  }
]
```

to add url to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/url",
    "value": "https://service-name.org/about"
  }
]
```

to add score to ldn notify service

```json
[
  {
    "op": "add",
    "path": "/score",
    "value": "0.89"
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
    "value": "https://service-name.org/ldn-inbox"
  }
]
```

to update the score of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/score",
    "value": "0.97"
  }
]
```

to update the url of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "/url",
    "value": "https://service-name.org/about"
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

to remove score value from ldn notify service

```json
[
  {
    "op": "remove",
    "path": "/score"
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
  "url" : "https://service-one.org/about",
  "ldnUrl" : "https://service-one.org/ldn-inbox",
  "score" : "0.57",
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
      "score" : "0.675",
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
      "url" : "https://service-two.org/about",
      "ldnUrl" : "https://service-two.org/ldn-inbox",
      "score" : "0.34",
      "enabled" : true,
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


## Notify Requests for specific item
### notifyrequests
**/api/ldn/notifyrequests/<:itemUUID>**

Parameters:
* The `itemUUID` is the uuid of the item

A sample search would be `/server/api/ldn/notifyrequests/_itemuuid_'

```json
{
    "notifyStatus": [
        {
            "serviceName": "NS",
            "serviceUrl": "2f4ec582-109e-4952-a94a-b7d7615a8c69",
            "status": "REJECTED"
        }
    ],
    "itemuuid": "_itemuuid_",
    "type": "notifyrequests",
    "_links": {
        "self": {
            "href": "http://localhost:8080/server/api/ldn/notifyrequests/ldn/org.dspace.app.rest.model.NotifyRequestStatusRest@31a86e42"
        }
    }
}
```

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the itemuuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden is not possible because it is restricted to authenticated users

