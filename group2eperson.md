# Group 2 EPerson mapping Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/group2eperson**

Filter using parameters:
* group : UUID of the parent group (mutually exclusive with eperson)
* eperson : UUID of the child eperson (mutually exclusive with group)
* all : retrieve idirect parent or child relations as well

Samples:
* GET /rest/api/eperson/group2eperson?group=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42
* GET /rest/api/eperson/group2eperson?group=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&all=true
* GET /rest/api/eperson/group2eperson?eperson=a6086b34-3918-45b7-8ddd-9329a702a26a
* GET /rest/api/eperson/group2eperson?eperson=a6086b34-3918-45b7-8ddd-9329a702a26a&all=true


```json
"_embedded": {
  "epersons": [
    {
      "id": "a6086b34-3918-45b7-8ddd-9329a702a26a",
      "uuid": "a6086b34-3918-45b7-8ddd-9329a702a26a",
      "name": "atmirenv@gmail.com",
      "handle": null,
      "metadata": [
        {
          "key": "eperson.firstname",
          "value": "Atmire",
          "language": null
        },
        {
          "key": "eperson.lastname",
          "value": "NV",
          "language": null
        },
        {
          "key": "eperson.language",
          "value": "en",
          "language": null
        }
      ],
      "netid": null,
      "lastActive": "2018-12-19T11:06:43.404+0000",
      "canLogIn": true,
      "email": "atmirenv@gmail.com",
      "requireCertificate": false,
      "selfRegistered": false,
      "groups": null,
      "type": "eperson",
      "_links": {
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/epersons/a6086b34-3918-45b7-8ddd-9329a702a26a"
        }
      }
    }
  ]
},
"_links": {
  "self": {
    "href": "https://dspace7-internal.atmire.com/rest/api/eperson/group2eperson?group=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42"
  }
},
"page": {
  "number": 0,
  "size": 10,
  "totalPages": 1,
  "totalElements": 10
}
}
```

## Add an EPerson to a parent Group

**POST /rest/api/eperson/group2eperson?group=<:uuid>&eperson=<:uuid>**

To add an eperson to a parent group, perform a post (no JSON) to the group2eperson endpoint when logged in as admin.

The parameters are mandatory:
* group : UUID of the parent group
* eperson : UUID of the child eperson

Sample:
* POST /rest/api/eperson/group2eperson?group=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&eperson=a6086b34-3918-45b7-8ddd-9329a702a26a

## Remove a Group from a parent Group

**DELETE /rest/api/eperson/group2eperson?group=<:uuid>&eperson=<:uuid>**

To remove an eperson to a parent group, perform a post (no JSON) to the group2group endpoint when logged in as admin.

The parameters are mandatory:
* group : UUID of the parent group
* eperson : UUID of the child eperson

Sample:
* DELETE /rest/api/eperson/group2eperson?group=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&eperson=a6086b34-3918-45b7-8ddd-9329a702a26a
