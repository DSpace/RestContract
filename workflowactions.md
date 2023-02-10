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

The **advanced** property on a workflow step is `false` by default. 
When it's true (when `advancedOptions` is not empty), the action has more advanced behavior than simple buttons. 
When this advanced action/option requires additional info about what to do from the rest, this will be put in `advancedInfo`
For new advanced steps this will require you to create an implementation of `ActionAdvancedInfo`, with corresponding REST & Angular classes.

Some samples are (see json of them below):
* `assign_eperson` will show an EPerson lookup on a workflow task page
* `rating` will show an option to enter a score on a workflow task page

Although `edit_metadata` is also advanced button with custom behavior, in the sense that the 'Edit' option/button in the second step of the regular workflow, will also open into a separate page, where you can edit the workflow item submission metadata fields (copy of submission form), a page which was already implemented before introduction of advanced workflow steps. The current implementation deviates too much to fit in this framework. In general we'll assume that an advanced workflow step will result in a page containing a simple metadata overview of the workflow item, preceded by a section containing the advanced workflow step actions, as supported/configured by the info in `advancedOptions`.

The **advancedOptions** property contains the list of actions which need the advanced functionality.

In some cases, the options may need extra info. The **advancedInfo** property contains that info, for the actions which require the extra details
These are added to rest via `ActionAdvancedInfo` and can be configured in `workflow-actions.xml`
```
    <bean id="scorereviewactionAPI" class="org.dspace.xmlworkflow.state.actions.processingaction.ScoreReviewAction" scope="prototype">
        <property name="descriptionRequired" value="true"/>
        <property name="maxValue" value="5"/>
    </bean>
```

Sample for selecting reviewer(s) who will perform a subsequent step:
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
