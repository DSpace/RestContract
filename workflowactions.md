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
  	    "submit_select_reviewer"
  	],
  	"advancedOptions": [
  	    "submit_select_reviewer"
  	],
  	"advancedInfo": [
      {
        "group": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
        "type": "action_info_submit_select_reviewer",
        "id": "b1d44ec2815cb282fd41146aab44967b"
      }
    ],
  	"type": "workflowaction"
}
```

To perform the action with selected reviewer(s) will be done by a request of the form (at least one eperson required):
```
POST /server/api/workflow/claimedtasks/{id} 
with form data 
submit_select_reviewer=true&eperson={reviewerEPersonId1}&eperson={reviewerEPersonId2}
```

Sample for entering a review with a score:
```json
{
  	"id": "scorereviewaction",
  	"advanced": "true",
  	"options": [
  	    "submit_score"
  	],
  	"advancedOptions": [
  	    "submit_score"
  	],
  	"advancedInfo": [
      {
        "descriptionRequired": "true",
        "maxValue": "5",
        "type": "action_info_submit_score",
        "id": "c969a40a9ebd3d08e210a5e59a4f4e0e"
      }
    ],
  	"type": "workflowaction"
}
```

To perform the action with selected reviewer(s) will be done by a request of the form (description optional):
```
POST /server/api/workflow/claimedtasks/{id} 
with form data
rating=true&score={scoreGivenValue}&description={reviewEntered}
```
