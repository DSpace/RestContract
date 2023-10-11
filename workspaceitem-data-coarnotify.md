# WorkspaceItem data of COAR Notify sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represents the data about the COAR Notify

```json
[
  {
    "pattern" : "endorsement",
    "services" : [ {
      "id" : 4,
      "name" : "service name two",
      "description" : null,
      "url" : null,
      "ldnUrl" : "service ldn url two",
      "notifyServiceInboundPatterns" : [ ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "ldnservice"
    }, {
      "id" : 5,
      "name" : "service name three",
      "description" : null,
      "url" : null,
      "ldnUrl" : "service ldn url three",
      "notifyServiceInboundPatterns" : [ ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "ldnservice"
    }]
  }, {
    "pattern" : "review",
    "services" : [ {
      "id" : 3,
      "name" : "service name one",
      "description" : null,
      "url" : null,
      "ldnUrl" : "service ldn url one",
      "notifyServiceInboundPatterns" : [ ],
      "notifyServiceOutboundPatterns" : [ ],
      "type" : "ldnservice"
    }]
  }
]
```

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body

### Add
To add a new services for a specific type of message

```json
[
  {
    "op": "add",
    "path": "/sections/coarnotify/review/-",
    "value": ["1","2","6"]
  }
]
```

### Replace
To replace a service for a specific type of message

```json
[
  {
    "op": "replace",
    "path": "/sections/coarnotify/endorsement/1",
    "value": "2"
  }
]
```

### Remove
It is possible to remove a previously service
`curl --data '{[ { "op": "remove", "path": "/sections/coarnotify/review/0"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
