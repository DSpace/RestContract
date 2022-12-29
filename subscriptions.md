# Subscriptions Endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains all of the subscription requested

## Main Endpoint
**GET /api/core/subscriptions?resourceType={resourceType}**

Retrieve a pageable list of subscriptions related with dspaceObject of type resourceType. 
The subscriptions are ordered by eperson number ascending.
The JSON response document is as follow.

```json
{
  "_embedded" : {
    "subscriptions" : [ {
      "id" : 1,
      "type" : "subscription",
      "subscriptionParameterList" : [ {
        "id" : 1,
        "name" : "Frequency",
        "value" : "Daily"
      } ],
      "subscriptionType" : "TypeTest",
      "_links" : {
        "dSpaceObject" : {
          "href" : "http://localhost/api/core/subscriptions/1/dSpaceObject"
        },
        "ePerson" : {
          "href" : "http://localhost/api/core/subscriptions/1/ePerson"
        },
        "self" : {
          "href" : "http://localhost/api/core/subscriptions/1"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/core/subscriptions"
    },
    "search" : {
      "href" : "http://localhost/api/core/subscriptions/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Attributes
* page, size [see pagination](README.md#Pagination)
* resourceType: If isn't defined it will return all the subscriptions.
  resourceType can be:
  * Item
  * Collection
  * Community

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated. Please note that this also apply to resource policy related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators, users with ADMIN right can access the subscriptions

## Single Subscription Object
**GET /api/core/subscription/<:id>**

Provide detailed information about a specific subscription.
The JSON response document is as follow

```
{
  "id" : 1,
  "type" : "subscription",
  "subscriptionParameterList" : [ {
    "id" : 1,
    "name" : "Frequency",
    "value" : "Daily"
  } ],
  "subscriptionType" : "TestType",
  "_links" : {
    "dSpaceObject" : {
      "href" : "http://localhost/api/core/subscriptions/1/dSpaceObject"
    },
    "ePerson" : {
      "href" : "http://localhost/api/core/subscriptions/1/ePerson"
    },
    "self" : {
      "href" : "http://localhost/api/core/subscriptions/1"
    }
  }
}
```

Status codes:
* 200 OK - if the subscription is found and it is visible to the current user.
* 401 Unauthorized - if you are not authenticated and the subscription is not visible to anonymous users
* 403 Forbidden - if you are not logged in with sufficient permissions. Only Admin and owner subscription has access.
* 404 Not found - if the subscription doesn't exist

### Search methods
#### findByEPerson
**GET /api/core/subscription/search/findByEPerson?uuid=<:ePerson-uuid>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* uuid: mandatory, the uuid of the eperson that benefit of content subscription

```json
{
  "_embedded": {
    "subscriptions": [
      {
        "id": 60,
        "type": "subscription",
        "subscriptionParameterList" : [ {
			    "id" : 2,
			    "name" : "Frequency",
			    "value" : "Daily"
		  } ],
        "subscriptionType": "subscription",
        "_links": {
          "dSpaceObject": {
            "href": "http://localhost:8080/server/api/core/subscriptions/60/dSpaceObject"
          },
          "ePerson": {
            "href": "http://localhost:8080/server/api/core/subscriptions/60/ePerson"
          },
          "self": {
            "href": "http://localhost:8080/server/api/core/subscriptions/60"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/core/subscriptions/search/findByEPerson?id=49e5ff01-5467-4acd-be21-9ad7374be929"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or the user specified in the uuid parameter can use the endpoint


#### findByEPersonAndDso
**GET /api/core/subscriptions/search/findByEPersonAndDso?dspace_object_id={id}&eperson_id={id}>**
Find all subscription that a person made for a dataSpaceObject

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* eperson_id: mandatory, the uuid of the eperson that benefit of content subscription
* dspace_object_id: mandatory, the uuid of the DSpaceObject (Community, Collection, Item)

```json
{
  "subscriptions": [
    {
      "id": 60,
      "type": "subscription",
      "subscriptionParameterList" : [ {
			    "id" : 12,
			    "name" : "Frequency",
			    "value" : "Daily"
		  } ],
      "subscriptionType": "subscription",
      "_links": {
        "dSpaceObject": {
          "href": "http://localhost:8080/server/api/core/subscriptions/60/dSpaceObject"
        },
        "ePerson": {
          "href": "http://localhost:8080/server/api/core/subscriptions/60/ePerson"
        },
        "self": {
          "href": "http://localhost:8080/server/api/core/subscriptions/60"
        }
      }
    }
  ],
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/core/subscriptions/search/findByEPerson?id=49e5ff01-5467-4acd-be21-9ad7374be929"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```
Status codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the dspace_object_id or eperson_id parameters are missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or the user specified in the eperson_id parameter can use the endpoint

## Creating a subscription
**POST /api/core/subscriptions?eperson_id=<:uuid>&dspace_object_id=<:uuid>**

Request body:
```json
{
  "subscriptionType": "Test",
  "subscriptionParameterList": [
    {
      "name": "Frequency",
      "value": "Daily"
    },
    {
      "name": "Frequency",
      "value": "WEEKLY"
    }
  ]
}
```
The parameters are:
eperson_id: (mandatory), the uuid of the eperson that will be grant of the subscription.
dspace_object_id: (mandatory), the uuid of the resource target of the subscription

Return codes:
* 200 OK - if the operation succeed, the created subscription is returned
* 400 Bad Request - if one or both the eperson_id and the eperson_id parameters are missing or aren't uuid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and Owner can create subscription

## Updating subscription
** PUT /api/core/subscription/<:id>**

It is possible to update a subscription with id
`curl -X PUT '{dspace7-url}/api/core/subscriptions/{id}?dspace_object_id={id}&eperson_id={id}
' -H "Authorization: Bearer ..." -H 'Content-Type: application/json'

```json
{
  "subscriptionType": "Test",
  "subscriptionParameterList": [
    {
      "name": "Frequency",
      "value": "Weekly"
    }
  ]
}
```

Return codes:
* 200 OK - if the operation succeed, the created subscription is returned
* 400 Bad Request - if one or both the eperson_id and the eperson_id parameters are missing or aren't uuid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and Owner can create subscription

## Deleting a Subscription Object
**DELETE /api/core/subscriptions/<:id>**

Deletes a subscription with id.

* 204 No content - if the operation succeed.
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions. It must be creator or administrator.
* 404 Not found - if the subscription with id doesn't exist (or was already deleted).

## Linked entities
### ePerson
**GET api/core/subscriptions/<:id>/ePerson**

Return the eperson linked by this subscription

Return codes:
* 200 OK - if the operation succeed
* 204 No content - if the operation succeed but no eperson is set for this policy
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions.
* 404 Not found - if the subscription doesn't exist (or was already deleted)

### dSpaceObject
**GET api/core/subscriptions/<:id>/dSpaceObject**

Return the DSpaceObject (Community, Collection, Item) linked by this subscription

Return codes:
* 200 Ok - if the operation succeed
* 204 No content - if the operation succeed but no eperson is set for this policy
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions.
* 404 Not found - if the subscription doesn't exist (or was already deleted)

