# EditItemMode Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Single EditItemMode
**/api/core/edititemmodes/<:item-uuid>:<:mode-name>**

Retrieves the configuration details for a specific edit mode applied to a specific item. Edit modes control how archived items can be edited, defining security constraints, which submission definition to use, and optional metadata filters for role-based editing.

Edit modes are **configuration-driven** (defined in `dspace/config/spring/api/edititem-service.xml`) and vary by entity type. They cannot be created, updated, or deleted via the REST API.

The endpoint uses a composite identifier `{itemUUID}:{modeName}` where:
- `itemUUID` - The UUID of the item being queried
- `modeName` - The name of the edit mode (e.g., "FULL", "OWNER", "INVESTIGATOR")

The JSON response document is as follows:
```json
{
  "id": "a1b2c3d4-5678-90ab-cdef-1234567890ab:OWNER",
  "name": "OWNER",
  "label": null,
  "submissionDefinition": "person-edit",
  "type": "edititemmode",
  "uniqueType": "core.edititemmode",
  "_links": {
    "self": {
      "href": "http://{dspace-url}/server/api/core/edititemmodes/a1b2c3d4-5678-90ab-cdef-1234567890ab:OWNER"
    }
  }
}
```

**Response fields:**
- `id`: Composite identifier in format `{itemUUID}:{modeName}`
- `name`: The mode name (matches the second part of the ID)
- `label`: Optional human-readable label for UI display (typically null)
- `submissionDefinition`: Name of the submission definition from `item-submission.xml` that defines the editing form/workflow for this mode

Return codes:
* 200 OK - if the mode exists and is configured for the item's entity type
* 400 Bad request - if the composite ID format is invalid (must be `{uuid}:{modeName}`)
* 401 Unauthorized - if you are not authenticated
* 404 Not found - if the item doesn't exist or the mode is not configured for the item's entity type

**Note:** This endpoint returns the mode configuration if it exists for the item's entity type, but does not verify whether the current user has permission to use that mode. Authorization is enforced when attempting to access or edit the item via `/api/core/edititems/{uuid}:{mode}`.
