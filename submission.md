# Submission / Deposit
[REST Overview Documentation](README.md)

One of the most used features in DSpace is the submission/workflow process.

Because of its configurability, many different REST endpoints are used during this process.
This page helps provide an overview of how to submit a new Item into DSpace.

**NOTE: All the activities below require authentication and a valid CSRF token.**
* Don't forget to include the "Authorization" header in every request! See [Authentication](authentication.md).
* Don't forget to include the CSRF token in every request! See [CSRF Tokens](csrf-tokens.md).

## Starting a Submission

In DSpace, a submission is started by creating a WorkspaceItem. The WorkspaceItem acts as an "in progress" submission object.

To create a WorkspaceItem, just `POST` to [/api/submission/workspaceitems](workspaceitems.md), passing in an owning Collection UUID (as every Item must belong to a Collection)
```
POST /api/submission/workspaceitems?owningCollection=<:collection-uuid>
```
This POST would create an empty WorkspaceItem by default.

Alternatively, you can create a WorkspaceItem by uploading a file via a Multipart POST,
or passing an [external entry value](external-authority-sources.md) whose metadata should be imported.
For more information, see "Multipart POST Method" in the [/api/submission/workspaceitems](workspaceitems.md) documentation.

## Modifying the Submission

Once a submission is created (see above), the response will provide a new [/api/submission/workspaceitems](workspaceitems.md) resource.
This is the WorkspaceItem object you created.

It is **important** to keep the `id` of the WorkspaceItem, as this is necessary to update it or access it again.
For example, using the `id`, you can load up the current state of your WorkspaceItem
```
GET /api/submission/workspaceitems/<:id>
```

In the response, you'll see a list of `sections` which are available to complete for this WorkspaceItem.
These `sections` correspond to [/api/config/submissionsections](submissionsections.md).
Each section name is also its `id`, so you can use the section name to get more information via the
that endpoint, e.g.
```
# Get info about the "traditionalpageone" section
GET /api/config/submissionsections/traditionalpageone
```
The `sectionType` returned in the response will tell you the type of section. This is important
in understanding how to interact with this section.

Each `sectionType` is separately described below, including notes on how to edit the section
* ["submission-form" section type](workspaceitem-data-metadata.md) - Used to enter metadata
* ["upload" section type](workspaceitem-data-upload.md) - Used to upload files
* ["access" section type](workspaceitem-data-access.md) - Used to edit access rights / embargo
* ["license" section type](workspaceitem-data-license.md) - Used to sign off on the required license agreement
* ["cclicense" section type](workspaceitem-data-cclicense.md) - Use to optionally apply a Creative Commons license

## Completing the Submission / Starting Workflow

In order to complete a submission, it MUST be sent into approval workflow. This is done by `POST`ing an existing
WorkspaceItem to the WorkflowItem endpoint.

See the "POST Method" of [/api/workflow/workflowitems](workflowitems.md) for examples.

In DSpace, approval workflow is configured per Collection, and is not enabled by default.
* If the Collection has no approval workflow configured (default), then the Item will become available immediately.
The final Item's UUID will be the same as it was in the WorkspaceItem (i.e. the `uuid` returned via
`/api/submission/workspaceitems/<:id>/item`)
* If the Collection has an approval workflow configured, then a WorkflowItem will be returned. Its `id` can be used
to access the WorkflowItem via `/api/workflow/workflowitems/<:id>`.

## Finding potential duplicate items

**GET /api/submission/duplicates/search?uuid=<:uuid>**

Provides a list of items that may be duplicates, if this feature is enabled, given the uuid as a parameter.

The potential duplicates listed in the section have all been detected by a special Solr search that compares the
levenshtein edit distance between the in-progress item title and other item titles (normalised).

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

See `dspace/config/modules/duplicate-detection.cfg` for configuration properties and examples.

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