# System-wide alerts
[Back to the list of all defined endpoints](endpoints.md)

This endpoint exposes system-wide alerts to be shown as banners in the frontend.
Only administrators are allowed to create and edit these alerts, while all other users can view them.

This feature will be expanded in the future
* DSpace currently only supports a single system-wide alert, but the endpoints are designed to accomodate multiple alerts
* Alerts can specify whether/how user sessions should be restricted while the alert is active, but this is a placeholder as session control has not been implemented yet.


## Retrieve all alerts
**GET /api/system/systemwidealerts**
This endpoint returns a list of all system-wide alerts.

Status codes:
* 200 OK - if the operation succeeded


## Retrieve a specific alert
**GET /api/system/systemwidealerts/<:alert-id>**
This endpoint returns a single system-wide alert by its integer identifier.

Status codes:
* 200 OK - if the operation succeeded 
* 404 Not found - if no alert exists for the requested identifier


## Create a new alert
**POST /api/system/systemwidealerts**
This endpoint allows administrators to create a new system-wide alert.

```json
{
  "id": 1,
  "message": "Alert message to show in the banner",
  "allowSessions": 2,
  "countdownTo": "2023-04-05T00:00:00.000Z",
  "active": true
}
```

The value of `countdownTo` must be a UTC datetime string.

The possible values for `allowSessions` are
* 0: allow all sessions
* 1: allow current sessions only (admins will still be allowed to log in)
* 2: allow admin sessions only (other users will be logged out)

Status codes:
* 201 Created - if the operation succeeded
* 400 Bad request - if a system-wide alert already exists in the database
* 422 Unprocessable Entity - if the request body cannot be parsed


## Modify an existing alert
**PUT /api/system/systemwidealerts/<:alert-id>**
This endpoint allows administrators to edit an existing system-wide alert.

```json
{
  "id": 1,
  "message": "Another message",
  "allowSessions": 0,
  "countdownTo": "2024-05-06T00:00:00.000Z",
  "active": true
}
```

The value of `countdownTo` must be a UTC datetime string.

The possible values for `allowSessions` are
* 0: allow all sessions
* 1: allow current sessions only (admins will still be allowed to log in)
* 2: allow admin sessions only (other users will be logged out)

Status codes:
* 200 OK - if the operation succeeded
* 422 Unprocessable entity - if the request body cannot be parsed
* 404 Not found - if no alert exists for the requested identifier
