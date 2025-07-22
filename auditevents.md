# Admin Audit Events Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint allows administrators to check the events recorded in the audit log.

## Main Endpoint
**GET /api/system/auditevents**

This endpoint will return a list of all the events recorded in the audit log.

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* sort, only timeStamp is supported

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access the audit events
* 404 Not Found - if the audit system is not enabled

## Single Event
**GET /api/system/auditevents/<:event-uuid>**

This endpoint will return details about the event

```json
{
  "id": "d7ca31fc-50a4-4a85-89ea-599fc1494f12",
  "epersonUUID": "685369f5-e169-48b3-bedc-70e5d03f8ce2",
  "objectUUID": null,
  "objectType": null,
  "subjectUUID": "e173a574-2de3-4f4a-843d-62fcfc4d2109",
  "subjectType": "COMMUNITY",
  "eventType": "MODIFY_METADATA",
  "timeStamp": "2025-07-21T14:49:54.718+00:00",
  "detail": null,
  "metadataField": "dc_identifier_uri",
  "value": "http://localhost:4000/handle/123456789/7",
  "authority": "",
  "confidence": -1,
  "place": 0,
  "action": "ADD",
  "checksum": null,
  "type": "auditevent",
  "_links": {
    "eperson": {
      "href": "http://localhost:8080/server/api/system/auditevents/d7ca31fc-50a4-4a85-89ea-599fc1494f12/eperson"
    },
    "object": {
      "href": "http://localhost:8080/server/api/system/auditevents/d7ca31fc-50a4-4a85-89ea-599fc1494f12/object"
    },
    "subject": {
      "href": "http://localhost:8080/server/api/system/auditevents/d7ca31fc-50a4-4a85-89ea-599fc1494f12/subject"
    },
    "self": {
      "href": "http://localhost:8080/server/api/system/auditevents/d7ca31fc-50a4-4a85-89ea-599fc1494f12"
    }
  }
}
```

## Search Events
**GET /api/system/auditevents/search/findByObject?object=<:uuid>**

This supports a basic search of events related to an object

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* sort, only timeStamp is supported
* object: mandatory, the uuid of the object that is involved in the Event (as subject or otherObjet)

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access the audit events
* 404 Not Found - if the audit system is not enabled