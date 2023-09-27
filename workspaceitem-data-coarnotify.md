# WorkspaceItem data of COAR Notify sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represents the data about the COAR Notify

```json
[
  {
    "pattern" : "review",
    "services" : [{
      "id" : 1,
      "name" : "service name",
      "description" : "service description"
    },
      {
        "id" : 2,
        "name" : "service name",
        "description" : "service description"
      }
    ]
  },
  {
    "pattern" : "endorsement",
    "services" : [{
      "id" : 3,
      "name" : "service name",
      "description" : "service description"
    }]
  },
  {
    "pattern" : "ingest",
    "services" : [{
      "id" : 4,
      "name" : "service name",
      "description" : "service description"
    }]
  }
]
```

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body

### Add
To add a new service for a specific type of message

```json
[
  {
    "op": "add",
    "path": "/sections/<:name-of-the-form>/review-service",
    "value": "1"
  }
]
```

### Replace
To replace a service for a specific type of message

```json
[
  {
    "op": "replace",
    "path": "/sections/<:name-of-the-form>/review-service",
    "value": "2"
  }
]
```

### Remove
It is possible to remove a previously service
`curl --data '{[ { "op": "remove", "path": "/sections/<:name-of-the-form>/review-service[0]"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
