# WorkspaceItem data of identifiers sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

This section data represent a list of potential duplicates associated for this workspace item.

It is a JSON object with the following structure (matches the response from the [duplicate search endpoint](duplicates.md)) :

```json
{
  "potentialDuplicates": [
    {
      "title": "Example Item",
      "uuid": "5ca83276-f003-460d-98b6-dd3c30708749",
      "owningCollectionName": "Publishers",
      "workspaceItemId": null,
      "workflowItemId": null,
      "metadata": {
        "dc.title": [
          {
            "value": "Example Item",
            "language": null,
            "authority": null,
            "confidence": -1,
            "place": 0
          }
        ],
        "dspace.entity.type": [
          {
          "value": "Publication",
          "language": null,
          "authority": null,
          "confidence": -1,
          "place": 0
        }
        ]
      },
      "type": "DUPLICATE"
    }, {
      "title": "Example Itom",
      "uuid": "32f8f6e4-c79e-4322-aae7-07ee535f70a6",
      "owningCollectionName": null,
      "workspaceItemId": 51,
      "workflowItemId": null,
      "metadata": {
        "dc.title": [{
          "value": "Example Itom",
          "language": null,
          "authority": null,
          "confidence": -1,
          "place": 0
        }]
      },
      "type": "DUPLICATE"
    }, {
      "title": "Exaple Item",
      "uuid": "0647ff45-48f5-4c1b-b6d7-f5dbbc160856",
      "owningCollectionName": null,
      "workspaceItemId": 52,
      "workflowItemId": null,
      "metadata": {
        "dc.title": [{
          "value": "Exaple Item",
          "language": null,
          "authority": null,
          "confidence": -1,
          "place": 0
        }]
      },
      "type": "DUPLICATE"
    }]
}
```
The potential duplicates listed in the section have all been detected by a special Solr search that compares the
 levenshtein edit distance between the in-progress item title and other item titles (normalised).

Each potential duplicate has the following attributes:

* title: The item title
* uuid: The item UUID
* owningCollectionName: Name of the owning collection, if present
* workspaceItemId: Integer ID of the workspace item, if present
* workflowItemId: Integer ID of the workflow item, if present
* metadata: A list of metadata values copied from the item, as per configuration
* type: The value is always DUPLICATE. This is the 'type' category used for serialization/deserialization.

See `dspace/config/modules/duplicate-detection.cfg` for configuration properties.

## Patch operations
There are no PATCH methods implemented for this section. 
