# WorkspaceItem data of COAR Notify sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represents the data about the selected COAR Notify patterns to request

```json
[
  {
    "endorsement": [ 1, 2, 3 ],
    "review": [ 6 ]
  }
]
```

the attributes represent the pattern that will be requested to the selected service(s), the value of each attribute is the array of the id of the selected services.
The above example shows a submission where three services (id 1, 2, 3) were selected to request an endorsement and one service (id 6) has been selected to request a review.

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the updated workspaceitem as body

### Add
To specify which services request for a specific pattern

```json
[
  {
    "op": "add",
    "path": "/sections/coarnotify/review/-",
    "value": [1, 2, 6]
  }
]
```

### Replace
To replace the 2nd previous selected service for the endorsement pattern

```json
[
  {
    "op": "replace",
    "path": "/sections/coarnotify/endorsement/1",
    "value": 4
  }
]
```

Please note that the number in the path (/1) represent the idx of the previous selected services, the number in the value represent the id of the new selected service. The above path applied to the initial sample
```json
[
  {
    "endorsement": [ 1, 2, 3 ],
    "review": [ 6 ]
  }
]
```

will modify the section data as follow

```json
[
  {
    "endorsement": [ 1, 4, 3 ],
    "review": [ 6 ]
  }
]
```

### Remove
It is possible to remove a previously selected service for a specific pattern (review)
`curl --data '{[ { "op": "remove", "path": "/sections/coarnotify/endorsement/0"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

The result applied to the previous example will be 

```json
[
  {
    "endorsement": [4, 3 ],
    "review": [ 6 ]
  }
]
```