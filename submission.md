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
GET /api/sumission/workspaceitems/<:id>
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
