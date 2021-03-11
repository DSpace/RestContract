# SubmissionUploads Endpoints
[Back to the list of all defined endpoints](endpoints.md)

The proposed structure is derived from the configuration options already available in DSpace 6 and below. In the first implementation, a single configuration named *default* is expected as these configurations were set at the site level. Introducing an endpoint to manage a collection of configurations we open the door to future extension where different setups can be used for different kind of submissions (theses, technical reports, journal articles, etc.)

## Main Endpoint
**/api/config/submissionuploads**   

Provide access to the existent configurations for the submission upload. It returns the list of existent SubmissionUploadConfig.
The SubmissionUploadConfig aggregates in a single place the configuration needed to the upload section such as the max allowed size for the files, the access condition options, etc.

Example: to be provided

## Single Submission-Form 
**/api/config/submissionuploads/<:upload-name>**

*:upload-name* is initially hard-coded to *default*

Provide detailed information about a specific input-form. The JSON response document is as follows:
```json
{
  "id": "default",
  "required": true,
  "max-size": 536870912,
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
The attributes of the objects in the accessConditionOptions arrays are as follow:
* The *name* attribute is an identifier that can be used by the REST client (the UI) to present the different options to the user, maybe translating it in an human readable format using i18n, and by the backend to create the ResourcePolicy to apply to the uploaded file using and validating the additional inputs provided by the user where required.
* If there is a *hasStartDate* attribute and it is true, the access condition to be applied requires to specify a startDate that must be less or equal than the value of the *maxStartDate* attribute (if null any date is acceptable)
* If there is a *hasEndDate* attribute and it is true, the access condition to be applied requires to specify an endDate that must be less or equal than the value of the *maxEndDate* attribute (if null any date is acceptable). If a startDate is supplied the endDate must be greater than the startDate

A null value for the accessConditionOptions attribute means that the upload step doesn't allow the user to set a policy for the file. The file will get only the policies inherited from the collection.

Exposed links:
* metadata: it is a link to the submission-form to use for the files metadata

### Linked entities
#### metadata
**/api/confg/submissionupload/:upload-name/metadata**
The [submission-form](submissionforms.md) used for the metadata of the files
