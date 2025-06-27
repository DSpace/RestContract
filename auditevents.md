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
  "id" : "954e5cfa-6990-4c85-ae42-f30d8c7888e2",
  "eventType" : "ADD",
  "timeStamp" : "2017-11-22T10:29:11Z",
  "detail" : "...",
  "subjectUUID" : "...uuid...",
  "subjectType" : "Collection",
  "objectUUID" : "...uuid...",
  "objectType" : "Item",
  "type" : "auditevent",
  "_links" : {
    "self" : {
      "href" : "/api/system/auditevents/954e5cfa-6990-4c85-ae42-f30d8c7888e2"
    },
    "eperson" : {
      "href" : "/api/system/auditevents/954e5cfa-6990-4c85-ae42-f30d8c7888e2/eperson"
    },
    "subject" : {
      "href" : "/api/system/auditevents/954e5cfa-6990-4c85-ae42-f30d8c7888e2/subject"
    },
    "object" : {
      "href" : "/api/system/auditevents/954e5cfa-6990-4c85-ae42-f30d8c7888e2/object"
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