# SubmissionAccessOptions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/config/submissionaccessoptions**   

Provide access to the existent configurations for the submission access. It returns the list of existent SubmissionAccessConfig.
The SubmissionAccessConfig aggregates in a single place the configuration needed to the access section.

## Single Submission-Form 
**/api/config/submissionaccessoptions/<:config-name>**

*:config-name* the access configuration name to retrieve

Provide detailed information about a specific input-form. The JSON response document is as follows:
```json
{
  "id": "default",
  "canChangeDiscoverable": true,
  "accessConditionOptions": [
		{
 			"name": "openaccess"
		},
		{
 			"name": "administrator"
		},  	 			
		{
 			"name": "embargo",
 			"hasStartDate": true,
 			"maxStartDate": "2018-06-24T00:40:54.970+0000"
		},
		{
 			"name": "lease",
 			"hasEndDate": true,
 			"maxEndDate": "2017-12-24T00:40:54.970+0000"
		}
  ]
}

```
The **canChangeDiscoverable** flag indicates whether or not the user has the opportunity to indicate that the current item must be findable via search or browse.

The attributes of the objects in the **accessConditionOptions** arrays are as follow:
* The *name* attribute is an identifier that can be used by the REST client (the UI) to present the different options to the user, maybe translating it in an human readable format using i18n, and by the backend to create the ResourcePolicy to apply to the accessed item using and validating the additional inputs provided by the user where required.
* If there is a *hasStartDate* attribute and it is true, the access condition to be applied requires to specify a startDate that must be less or equal than the value of the *maxStartDate* attribute (if null any date is acceptable)
* If there is a *hasEndDate* attribute and it is true, the access condition to be applied requires to specify an endDate that must be less or equal than the value of the *maxEndDate* attribute (if null any date is acceptable). If a startDate is supplied the endDate must be greater than the startDate

A null value for the accessConditionOptions attribute means that the access step doesn't allow the user to set a policy for the item. The item will get only the policies inherited from the collection.
