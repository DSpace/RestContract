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

-adding object to the last, this array isn't sorted so, not supported adding to index like "notifyservices_inbound_patterns[index]"

```json
[
  {
    "op": "add",
    "path": "notifyservices_inbound_patterns/-",
    "value": {"pattern":"patternA","constraint":"itemFilterA","automatic":"false"}
  }
]
```

to add outboundPatterns to ldn notify service

-adding object to the last, this array isn't sorted so, not supported adding to index like "notifyservices_outbound_patterns[index]"

```json
[
  {
    "op": "add",
    "path": "notifyservices_outbound_patterns/-",
    "value": {"pattern":"patternA","constraint":"itemFilterA"}
  }
]
```

to add pattern to specific inboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyservices_inbound_patterns[0]/pattern",
    "value": "patternA"
  }
]
```

to add constraint to specific inboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyservices_inbound_patterns[0]/constraint",
    "value": "itemFilterA"
  }
]
```


to add pattern to specific outboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyservices_outbound_patterns[0]/pattern",
    "value": "patternA"
  }
]
```

to add constraint to specific outboundPattern of ldn notify service

```json
[
  {
    "op": "add",
    "path": "notifyservices_outbound_patterns[0]/constraint",
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

to replace all inboundPatterns of ldn notify service,
if value contains an empty array all inboundPatterns will be removed

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns",
    "value": [{"pattern":"patternC","constraint":"itemFilterC","automatic":"false"}, {"pattern":"patternD","constraint":"itemFilterD","automatic":"false"}]
  }
]
```

to replace inboundPattern of ldn notify service at specific index

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns[0]",
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
    "path": "notifyservices_outbound_patterns",
    "value": [{"pattern":"patternC","constraint":"itemFilterC"}, {"pattern":"patternD","constraint":"itemFilterD"}]
  }
]
```

to replace outboundPattern of ldn notify service at specific index

```json
[
  {
    "op": "replace",
    "path": "notifyservices_outbound_patterns[0]",
    "value": {"pattern":"patternD","constraint":"itemFilterD"}
  }
]
```

to replace pattern of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns[1]/pattern",
    "value": "patternB"
  }
]
```

to replace constraint of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns[1]/constraint",
    "value": "itemFilterB"
  }
]
```

to replace automatic of specific inboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyservices_inbound_patterns[1]/automatic",
    "value": "true"
  }
]
```

to replace pattern of specific outboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyservices_outbound_patterns[1]/pattern",
    "value": "patternB"
  }
]
```

to replace constraint of specific outboundPattern of ldn notify service

```json
[
  {
    "op": "replace",
    "path": "notifyservices_outbound_patterns[0]/constraint",
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
    "path": "notifyservices_inbound_patterns"
  }
]
```

to remove inboundPattern from ldn notify service at specific index

```json
[
  {
    "op": "remove",
    "path": "notifyservices_inbound_patterns[0]"
  }
]
```

to remove all outboundPatterns of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyservices_outbound_patterns"
  }
]
```

to remove outboundPattern from ldn notify service at specific index

```json
[
  {
    "op": "remove",
    "path": "notifyservices_outbound_patterns[1]"
  }
]
```

to remove constraint from specific inboundPattern of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyservices_inbound_patterns[0]/constraint"
  }
]
```

to remove constraint from specific outboundPattern of ldn notify service

```json
[
  {
    "op": "remove",
    "path": "notifyservices_outbound_patterns[0]/constraint"
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