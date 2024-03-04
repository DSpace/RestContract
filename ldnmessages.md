# LDN Messages Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/ldn/ldnmessages**

Provide access to the ldn messages. List all the LDN messages

A sample can be found at https://api7.dspace.org/server/#/server/api/ldn/ldnmessages

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access

## Single LDN Message
**/api/ldn/ldnmessages/<:id>**

```json
{
  "id" : "urn:uuid:94ecae35-dcfd-4182-8550-22c7164fe23f",
  "notificationId" : "urn:uuid:94ecae35-dcfd-4182-8550-22c7164fe23f",
  "queueStatus" : 1,
  "queueStatusLabel" : "QUEUE_STATUS_QUEUED",
  "context" : "7d95587b-8e26-4302-9911-3e0f40fc6e4b",
  "object" : "7d95587b-8e26-4302-9911-3e0f40fc6e4b",
  "target" : null,
  "origin" : 1,
  "inReplyTo" : null,
  "activityStreamType" : "Announce",
  "coarNotifyType" : "coar-notify:EndorsementAction",
  "queueAttempts" : 0,
  "queueLastStartTime" : null,
  "queueTimeout" : "2024-01-03T10:56:41.737+00:00",
  "notificationType" : "Incoming",
  "type" : "message",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/ldn/messages/urn:uuid:94ecae35-dcfd-4182-8550-22c7164fe23f"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access
* 404 Not found - if no LDN Message exists with such id

## Creating a new LDN Message
**POST /api/ldn/ldnmessages**

Creation of LDN Message is not supported via Endpoint

Status codes:
* 405 Method Not Allowed

## PATCH

Patch of LDN Message is not supported via Endpoint

Status codes:
* 405 Method Not Allowed

## DELETE

Deletion of LDN Message is not supported via Endpoint

Status codes:
* 405 Method Not Allowed

## enqueueRetry
**POST /api/ldn/ldnmessages/<:id>/enqueueretry**

this endpoint is responsible for requesting a reprocessing of LDN Message
by changing `queueStatusLabel` to be "QUEUE_STATUS_QUEUED_FOR_RETRY"
```json
{
  "id" : "urn:uuid:94ecae35-dcfd-4182-8550-22c7164fe23f",
  "notificationId" : "urn:uuid:94ecae35-dcfd-4182-8550-22c7164fe23f",
  "queueStatus" : 7,
  "queueStatusLabel" : "QUEUE_STATUS_QUEUED_FOR_RETRY",
  "context" : "c433ac1a-949a-4435-bf3d-181dcaac1c56",
  "object" : "c433ac1a-949a-4435-bf3d-181dcaac1c56",
  "target" : null,
  "origin" : 1,
  "inReplyTo" : null,
  "activityStreamType" : "Announce",
  "coarNotifyType" : "coar-notify:EndorsementAction",
  "queueAttempts" : 0,
  "queueLastStartTime" : null,
  "queueTimeout" : 1704281265044,
  "notificationType" : "Incoming",
  "type" : "message"
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators can access
* 404 Not found - if no LDN Message exists with such id
