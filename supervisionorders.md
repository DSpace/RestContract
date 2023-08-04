# Supervision Orders Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint is used to create, handle and find supervision orders.

## Main Endpoint
**/api/core/supervisionorders**

Provides paginated information about all supervision orders defined in the system. See next paragraph for details about how supervision orders information is displayed.

## Single Supervision Order
**/api/core/supervisionorders/<:id>**

Provides information about a single supervision order defined into the system

```json
{
  "id": 42,
  "_links": {
    "item": {
      "href": "https://api7.dspace.org/server/api/core/items/092b59e8-8159-4e70-98b5-93ec60bd3431"
    },
    "group": {
      "href": "https://api7.dspace.org/server/api/eperson/groups/4ebb837c-c2ae-4928-9bb1-6f51df4eeb60"
    }
  }
```
Supervision order properties:
* id: the identifier assigned to the supervision order

Exposed links:
* item: the item on which this supervision order is defined
* group: the group that can supervise the item

Status codes:
* 200 OK - if the item is found and it is visible to the current user.
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions (Administrators).
* 404 Not found - if the supervision order doesn't exist


## Search methods

### Find by Item
**/api/core/supervisionorders/search/byItem?uuid=<:item-uuid1>**

This method returns a list of supervision orders defined for an item whose uuid is passed as parameter to the query

The supported parameters are:
* uuid: mandatory. The uuid of the item for which supervision order must be found

 
Return codes:
* 200 OK - if the operation succeed. Including the case where item exists but does not have any supervision order defined.
* 400 Bad Request - if uuid parameter is missing or syntactically invalid (not an uuid) 
* 404 Not found - if no items are found for the uuid..

## Creating a supervision order

**POST /api/core/supervisionorders?uuid=<:item uuid>&group=<:group uuid>&type=<:type>**

The supported parameters are:
* uuid: mandatory. The uuid of the item for which supervision order must be created
* group: mandatory. The uuid of the group whose members will be supervisors
* type: mandatory. The type of permissions to be granted to above group's members: 
  * NONE: no grants
  * EDITOR: READ and WRITE permissions will be added to item and its bitstreams
  * OBSERVER: READ permissions will be added to item and its bitstreams

A sample CURL command would be:
```
curl -i -X POST 'https://api7.dspace.org/server/api/core/supervisionorders?uuid=<uuid>&group=<group uuid>&type=NONE' -H 'Authorization: Bearer eyJrasdfw…' 
```

* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 400 Bad Request - if both or one among the uuid or group parameter is missing or syntactically invalid (not an uuid), or if they resolve to an unexpected DSpace Object type.
* 403 Forbidden - if you are not logged in with sufficient permissions (Administrator)
* 422 Unprocessable Entity - if one among item or group does not exist, if a supervision order for the same group already exists, or if the item referenced by uuid is not an inprogress submission. A Supervision order can be created only when the item is in the submission or workflow process


## Deleting a supervision order

**DELETE /api/core/supervisionorders/<:id>**

Delete a supervision order.

A sample CURL command would be:
```
curl -D - -XDELETE 'https://api7.dspace.org/server/api/core/supervisionorders/42'  -H 'Authorization: Bearer eyJhafsdf…'
```

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions (Administrator)
* 404 Not found - if the supervision order doesn't exist 
