# Subscriptions Endpoints

[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains all of the subscription requested

## Main Endpoint
**GET /api/core/subscriptions**

Retrieve a pageable list of subscriptions. 
The subscriptions are ordered by DSpaceObject uuid ascending.
The JSON response document is as follow.

```json
{
  "_embedded" : {
    "subscriptions" : [ {
      "id" : 1,
      "type" : "subscription",
      "subscriptionParameterList" : [ {
        "name" : "frequency",
        "value" : "D"
      } ],
      "subscriptionType" : "content",
      "_links" : {
        "resource" : {
          "href" : "http://localhost/api/core/subscriptions/1/resource"
        },
        "eperson" : {
          "href" : "http://localhost/api/core/subscriptions/1/eperson"
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

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators can access the endpoint

## Single Subscription Object
**GET /api/core/subscription/<:id>**

Provide detailed information about a specific subscription.
The JSON response document is as follow

```
{
  "id" : 1,
  "type" : "subscription",
  "subscriptionParameterList" : [ {
    "name" : "frequency",
    "value" : "W"
  } ],
  "subscriptionType" : "content",
  "_links" : {
    "resource" : {
      "href" : "http://localhost/api/core/subscriptions/1/resource"
    },
    "eperson" : {
      "href" : "http://localhost/api/core/subscriptions/1/eperson"
    },
    "self" : {
      "href" : "http://localhost/api/core/subscriptions/1"
    }
  }
}
```

JSON description:
- subscriptionType: currently only content but open to future extensions like statistics
- subscriptionParamterList: where the "frequency" is the currently only supported name and the different values ('D' stand for Day, 'W' stand for Week and 'M' stand for Month).

Status codes:
* 200 OK - if the subscription is found and current user is Admins or owner of the subscription.
* 401 Unauthorized - if you are not authenticated. Only Admins and owner of the subscription has access.
* 403 Forbidden - if you are not logged in with sufficient permissions. Only Admins and owner of the subscription has access.
* 404 Not found - if the subscription doesn't exist.

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
			    "name" : "frequency",
			    "value" : "M"
		  } ],
        "subscriptionType": "content",
        "_links": {
          "resource": {
            "href": "http://localhost:8080/server/api/core/subscriptions/60/resource"
          },
          "eperson": {
            "href": "http://localhost:8080/server/api/core/subscriptions/60/eperson"
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
* 422 Unprocessable Entity - if the uuid resolve to a different DSpace Object Type than eperson
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or the user specified in the uuid parameter can use the endpoint


#### findByEPersonAndDso
**GET /api/core/subscriptions/search/findByEPersonAndDso?resource={id}&eperson_id={id}>**
Find all subscription that a person made for a dataSpaceObject

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* eperson_id: mandatory, the uuid of the eperson that benefit of content subscription
* resource: mandatory, the uuid of the DSpaceObject (Community, Collection, Item)

```json
{
  "subscriptions": [
    {
      "id": 60,
      "type": "subscription",
      "subscriptionParameterList" : [ {
			    "name" : "frequency",
			    "value" : "D"
		  } ],
      "subscriptionType": "content",
      "_links": {
        "resource": {
          "href": "http://localhost:8080/server/api/core/subscriptions/60/resource"
        },
        "eperson": {
          "href": "http://localhost:8080/server/api/core/subscriptions/60/eperson"
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
* 400 Bad Request - if the resource or eperson_id parameters are missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators or the user specified in the eperson_id parameter can use the endpoint

## Creating a subscription
**POST /api/core/subscriptions?eperson_id=<:uuid>&resource=<:uuid>**

Request body:

```json
{
  "subscriptionType": "content",
  "subscriptionParameterList": [
    {
      "name": "frequency",
      "value": "D"
    },
    {
      "name": "frequency",
      "value": "W"
    }
  ]
}
```
The parameters are:
eperson_id: (mandatory), the uuid of the eperson that will be grant of the subscription.
resource: (mandatory), the uuid of the resource target of the subscription

The json body must be valid that mean:
- subscriptionType must be 'content'
- name must be 'frequency'
- value must be one of the following values 'D' stand for Day, 'W' stand for Week and 'M' stand for Month

Return codes:
* 200 OK - if the operation succeed, the created subscription is returned
* 400 Bad Request - if one or both the resource and the eperson_id parameters are missing or aren't uuid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and Owner can create subscription
* 422 Unprocessable Entity - if the subscriptionType or subscriptionParameter name or value are invalid

## Updating subscription
** PUT /api/core/subscription/<:id>**

It is possible to update a subscription with id
`curl -X PUT '{dspace7-url}/api/core/subscriptions/{id}
' -H "Authorization: Bearer ..." -H 'Content-Type: application/json'

```json
{
  "subscriptionType": "content",
  "subscriptionParameterList": [
    {
      "name": "frequency",
      "value": "M"
    }
  ]
}
```

The json body must be valid that mean:
- subscriptionType must be 'content'
- name must be 'frequency'
- value must be one of the following values: 'D' stand for Day, 'W' stand for Week and 'M' stand for Month

Return codes:
* 200 OK - if the operation succeed, the created subscription is returned
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators and Owner can create subscription
* 422 Unprocessable Entity - if the subscriptionType or subscriptionParameter name or value are invalid

## Deleting a Subscription Object
**DELETE /api/core/subscriptions/<:id>**

Deletes the subscription with the specified id.

* 204 No content - if the operation succeed.
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions. It must be owner of the subscription or an administrator.
* 404 Not found - if the subscription with id doesn't exist (or was already deleted).

## Linked entities
### eperson
**GET api/core/subscriptions/<:id>/eperson**

Return the eperson linked by this subscription

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in with sufficient permissions.
* 404 Not found - if the subscription doesn't exist (or was already deleted)

### resource
**GET api/core/subscriptions/<:id>/resource**

Return the DSpaceObject (Community, Collection, Item) linked by this subscription

Return codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions.
* 404 Not found - if the subscription doesn't exist (or was already deleted)

