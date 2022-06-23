# ORCID Queue Endpoints
[Back to the list of all defined endpoints](endpoints.md)

These endpoints allow to handle the Orcid Queue records associated with a specific profile.
Only the EPerson who is the owner of the Profile Item can access them.

When these records are processed lead to the creation of Orcid History records
(see [orcid history contract](orcidhistories.md) for more details). 

These endpoints are only available when orcid.sychronization-enabled=true. 

## Single ORCID Queue entry
**GET /api/eperson/orcidqueues/<:id>**

Provide detailed information about a specific ORCID queue entry. The JSON response document is as follow
```json
{
  "id": 73,
  "profileItemId": "87d24f06-0ca5-4abe-84f9-bfd36a8236d8",
  "entityId": "66e0d773-ddeb-4b6b-bcac-ced2ae67141c",
  "description": "My Publication",
  "recordType": "Publication",
  "operation": "INSERT",
  "type": "orcidqueue",
  "_links": {
    "self": {
      "href": "**/api/eperson/orcidqueues/73"
    }
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions  (only the owner of the profile is allowed)
* 404 Not found - if an ORCID queue entry with the given id doesn't exist

The `profileItemId` attribute represent the id of the researcher profile item that is associated with the the entity item with id `entityId` that should be sent to the ORCID register.

## Search ORCID Queue entries by profileItem id
**GET /api/eperson/orcidqueues/search/findByProfileItem?profileItemId=<:item-uuid>**

Provide detailed information about all the ORCID queue entries related to the profileItem with the given uuid. The JSON response document is as follow
```json
{
  "_embedded": {
    "orcidQueues": [
      {
        "id": 68,
        "profileItemId": "87d24f06-0ca5-4abe-84f9-bfd36a8236d8",
        "entityId": "3eed1cf2-552d-491a-9a1f-ef941a0cafcd",
        "description": "ABC-54321",
        "recordType": "Project",
        "operation": "UPDATE",
        "type": "orcidQueue",
        "_links": {
          "self": {
            "href": "**/api/eperson/orcidqueues/68"
          }
        }
      },
      {
        "id": 73,
        "profileItemId": "87d24f06-0ca5-4abe-84f9-bfd36a8236d8",
        "entityId": "66e0d773-ddeb-4b6b-bcac-ced2ae67141c",
        "description": "My Publication",
        "recordType": "Publication",
        "operation": "DELETE",
        "type": "orcidQueue",
        "_links": {
          "self": {
            "href": "**/api/eperson/orcidqueues/73"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/eperson/orcidQueues/search/findByProfileItem"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions (only the owner of the profile is allowed)

## Delete an ORCID Queue entry
**DELETE /api/eperson/orcidqueues/<:id>**

Delete a single ORCID queue entry by id. The provided response has no content.

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions  (only the owner of the profile is allowed)
