# Workflow Actions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A workflow action represents a single action in the reviewing process, e.g. the accept/reject action.  

All endpoints mentioned here require authentication, but no specific permissions.

## Main Endpoint
**/api/config/workflowactions**   

The main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single Workflow Action Definition
**/api/config/workflowactions/<:action-name>**

Provide detailed information about a specific workflow action. An example JSON response document:
```json
{
  	"id": "editaction",
  	"advanced": "false",
  	"options": [
  	    "approve",
  	    "reject",
  	    "edit_metadata"
  	],
  	"type": "workflowaction"
}
```

The **options** property contains the list of actions the user is authorized to perform in this step:
* The **edit_metadata** option implies the user can use the PATCH on the workflow item's submission sections to edit the metadata.
* Other options are considered to be command options sent to REST using a [POST to the claimed task](claimedtasks.md#post-method-single-resource-level)

The **advanced** property is `false` by default. When it's true, the action has more advanced behavior than simple buttons. Some samples are:
* `edit_metadata` will redirect the user to the edit page
* `assign_eperson` will show an EPerson lookup on a workflow task page
* `rating` will show an option to enter a score on a workflow task page

Although `edit_metadata` is also an advanced button with custom behavior, the current implementation deviates too much to fit in this framework.

The **advancedOptions** property contains the list of actions which need the advanced functionality.

In some cases, the options may need extra info. The **advancedInfo** property contains that info, for the actions which require the extra details

Sample for selecting reviewers who will perform a subsequent step:
```json
{
  	"id": "selectrevieweraction",
  	"advanced": "true",
  	"options": [
  	    "assign_eperson"
  	],
  	"advancedOptions": [
  	    "assign_eperson"
  	],
  	"advancedInfo": [
      {
        "group": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
        "minPeople": "2",
        "maxPeople": "4",
        "type": "action_info_assign_eperson",
        "id": "b1d44ec2815cb282fd41146aab44967b"
      }
    ],
  	"type": "workflowaction"
}
```

Sample for entering a review with a score:
```json
{
  	"id": "scorereviewaction",
  	"advanced": "true",
  	"options": [
  	    "rating"
  	],
  	"advancedOptions": [
  	    "rating"
  	],
  	"advancedInfo": [
      {
        "descriptionRequired": "true",
        "maxValue": "5",
        "type": "action_info_rating",
        "id": "c969a40a9ebd3d08e210a5e59a4f4e0e"
      }
    ],
  	"type": "workflowaction"
}
```
