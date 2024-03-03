# Duplicate detection endpoint
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/submission/duplicates**

Provide access to basic duplicate detection services. These services use Solr and the levenshtein distance operator
to detect potential duplicates of a given item, useful during submission and workflow review.

See `dspace/config/modules/duplicate-detection.cfg` for configuration properties and examples.

## Single duplicate

Not implemented. (a duplicate only makes sense in the context of a search by item)

## All duplicates

Not implemented. (a duplicate only makes sense in the context of a search by item)

## Search

**GET /api/submission/duplicates/search/findByItem?uuid=<:uuid>**

Provides a list of items that may be duplicates, if this feature is enabled, given the uuid as a parameter.

Note that although this appears in the submission category, the item UUID can also be an archived item.
Currently, the only frontend use of this feature is in workspace and workflow, so it is categorised as such.

Each potential duplicate has the following attributes:

* title: The item title
* uuid: The item UUID
* owningCollectionName: Name of the owning collection, if present
* workspaceItemId: Integer ID of the workspace item, if present
* workflowItemId: Integer ID of the workflow item, if present
* metadata: A list of metadata values copied from the item, as per configuration
* type: The value is always DUPLICATE. This is the 'type' category used for serialization/deserialization.

Example

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