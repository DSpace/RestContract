# WorkspaceItem data of identifiers sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the metadata collected for a specific [submissionform](submissionforms.md).
It is a json object with the following structure:

```json
"identifiers" : {
  "handle" : "http://localhost:4000/handle/123456789/6",
  "doi" : "https://doi.org/10.5072/dspace/2",
  "otherIdentifiers" : [ ]
},
```

Please note that the identifier section will never be empty. If there is no handle, the handle value will be null. If there is no DOI, the doi value will be null. If there are no other identifiers, the otherIdentifiers value will be an empty array.

## Patch operations
There are no PATCH methods implemented for this section, it simply retrieves and reports identifiers currently assigned to the workspace item.

### Remove
To limit the load on the Sherpa webservices and improve performance sherpa data are cached. It is possible to force a fresh call removing the retrievalTime from the section as follow
`curl --data '[{ "op": "remove", "path": "/sections/sherpaPolicies/retrievalTime"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

