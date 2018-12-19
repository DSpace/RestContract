# Group 2 Group mapping Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/group2group**

Filter using parameters:
* parent : UUID of the parent group (mutually exclusive with child)
* child : UUID of the child group (mutually exclusive with parent)
* all : retrieve idirect parent or child relations as well

Samples:
* GET /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42
* GET /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&all=true
* GET /rest/api/eperson/group2group?child=ddca960c-2033-4cf0-9430-5918d94751ef
* GET /rest/api/eperson/group2group?child=ddca960c-2033-4cf0-9430-5918d94751ef&all=true

```json
{
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/groups"
    }
  },
  "_embedded": {
    "groups": [
      {
        "id": "ddca960c-2033-4cf0-9430-5918d94751ef",
        "uuid": "ddca960c-2033-4cf0-9430-5918d94751ef",
        "name": "COLLECTION_1_WORKFLOW_ROLE_reviewer",
        "handle": null,
        "metadata": [
          {
            "key": "dc.title",
            "value": "COLLECTION_1_WORKFLOW_ROLE_reviewer",
            "language": null
          }
        ],
        "permanent": false,
        "type": "group",
        "_links": {
          "groups": {
            "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef/groups"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef"
          }
        },
        "_embedded": {
          "groups": {
            "_embedded": {
              "groups": []
            },
            "_links": {
              "self": {
                "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef/groups"
              }
            },
            "page": {
              "number": 0,
              "size": 0,
              "totalPages": 1,
              "totalElements": 0
            }
          }
        }
      },
      {
        "id": "1cbf084f-a3d1-423c-949c-b5ae96859d8b",
        "uuid": "1cbf084f-a3d1-423c-949c-b5ae96859d8b",
        "name": "COLLECTION_16_WORKFLOW_ROLE_reviewer",
        "handle": null,
        "metadata": [
          {
            "key": "dc.title",
            "value": "COLLECTION_16_WORKFLOW_ROLE_reviewer",
            "language": null
          }
        ],
        "permanent": false,
        "type": "group",
        "_links": {
          "groups": {
            "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b/groups"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b"
          }
        },
        "_embedded": {
          "groups": {
            "_embedded": {
              "groups": []
            },
            "_links": {
              "self": {
                "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b/groups"
              }
            },
            "page": {
              "number": 0,
              "size": 0,
              "totalPages": 1,
              "totalElements": 0
            }
          }
        }
      }
    ]
  },
  "page": {
    "number": 0,
    "size": 2,
    "totalPages": 1,
    "totalElements": 2
  }
}
```

## Add a Group to a parent Group

**POST /rest/api/eperson/group2group?parent=<:uuid>&child=<:uuid>**

To add a child group to a parent group, perform a post (no JSON) to the group2group endpoint when logged in as admin.

The parameters are mandatory:
* parent : UUID of the parent group
* child : UUID of the child group

Sample:
* POST /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&child=ddca960c-2033-4cf0-9430-5918d94751ef

## Remove a Group from a parent Group

**DELETE /rest/api/eperson/group2group?parent=<:uuid>&child=<:uuid>**

To remove a child group to a parent group, perform a post (no JSON) to the group2group endpoint when logged in as admin.

The parameters are mandatory:
* parent : UUID of the parent group
* child : UUID of the child group

Sample:
* DELETE /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&child=ddca960c-2033-4cf0-9430-5918d94751ef
