# BulkAccessConditionOptions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/config/bulkaccessconditionoptions**   

Provide access to the existent configurations for the bulk access control management. It returns the list of existent BulkAccessConfig.
The BulkAccessConfig aggregates in a single place the configuration needed to the access section of the bulk access control management at the item and bitstream level.

Please note that currently only a single repository wide configuration is supported and it is configured in the backend `/config/spring/api/access-conditions.xml` file, so this endpoint will list a single configuration.

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. This endpoint is only accessible ot user that has administrative priviled over at least one community, collection or item limited to the resources that they admin.

## Single bulkaccessconditionoptions-Form 
**/api/config/bulkaccessconditionoptions/<:config-name>**

*:config-name* the bulk access control management configuration name to retrieve

Please note that currently only a single repository wide configuration is supported and it is named `default`

Provide detailed information about a specific input-form. The JSON response document is as follows:
```json
{
  "id": "default",
  "itemAccessConditionOptions": [
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
  ],
  "bitstreamAccessConditionOptions": [
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

The attributes of the objects in the **itemAccessConditionOptions** and **bitstreamAccessConditionOptions** arrays are as follow:
* The *name* attribute is an identifier that can be used by the REST client (the UI) to present the different options to the user, maybe translating it in an human readable format using i18n, and by the backend to create the ResourcePolicy to apply to the accessed item using and validating the additional inputs provided by the user where required.
* If there is a *hasStartDate* attribute and it is true, the access condition to be applied requires to specify a startDate that must be less or equal than the value of the *maxStartDate* attribute (if null any date is acceptable)
* If there is a *hasEndDate* attribute and it is true, the access condition to be applied requires to specify an endDate that must be less or equal than the value of the *maxEndDate* attribute (if null any date is acceptable). If a startDate is supplied the endDate must be greater than the startDate

Obviously, **itemAccessConditionOptions** provides the list of options acceptable at the item level whereas **bitstreamAccessConditionOptions** provides the list of options suitable at the bitstream level

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. This endpoint is only accessible ot user that has administrative priviled over at least one community, collection or item limited to the resources that they admin.
* 404 Not Found - if you request any configuration other than `default`
