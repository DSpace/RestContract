# WorkspaceItem data of identifiers sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

This section data represent the identifiers associated for this workspace item.

It is a JSON object with the following structure (matches the response from the [item identifier link](items.md#Get and create item identifiers)) :

```json
 {
  "identifiers": {
    "identifiers": [
      {
        "value": "https://doi.org/10.33515/dspace-54",
        "identifierType": "doi",
        "identifierStatus": "PENDING",
        "type": "identifier"
      },
      {
        "value": "http://localhost:4000/handle/123456789/420",
        "identifierType": "handle",
        "identifierStatus": null,
        "type": "identifier"
      }
    ],
    "displayTypes": [
      "handle",
      "doi"
    ]
  }
}
```
Each identifier has the following attributes:
* value: The actual identifier string, eg. https://doi.org/2312/23123.22
* identifierType: The type or schema of identifier, eg, 'doi' or 'handle'
* identifierStatus: The status of an identifier, if relevant (some identifier types require values to be locally minted and then reserved or registered with an upstream provider)
* type: The type of resource, to help preserve context in the user interface

Currently, the supported item types are 'doi' and 'handle'

## Patch operations
There are no PATCH methods implemented for this section, it simply retrieves and reports identifiers currently assigned to the workspace item.
