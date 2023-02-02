# System-wide alerts
[Back to the list of all defined endpoints](endpoints.md)

This endpoint exposes system-wide alerts to be shown as banners in the frontend.
Only administrators are allowed to create and edit these alerts, while all other users can view them.
Alerts are only shown to regular users when set to "active"; only administrators can see inactive alerts.

This feature will be expanded in the future
* DSpace currently only supports a single system-wide alert, but the endpoints are designed to accomodate multiple alerts
* Alerts can specify whether/how user sessions should be restricted while the alert is active, but this is a placeholder as session control has not been implemented yet.

## Retrieve all active alerts
**GET /api/system/systemwidealerts/search/active**
Returns all currently active alerts; used to determine which alerts to show in the UI.
This is the only endpoint non-admin users ever need to interact with.

Status codes:
* 200 OK - if the operation succeeded


## Retrieve a specific alert
**GET /api/system/systemwidealerts/<:alert-id>**
This endpoint returns a single system-wide alert by its integer identifier.
If the alert in question is inactive, it will only be exposed to admin users. 
For other users it will appear as if no alert exists for the given identifier.

Example response:
```json
{
  "alertId" : 1,
  "message" : "This message will show up in the banner",
  "allowSessions" : "all",
  "countdownTo" : "2023-04-05T00:00:00.000+00:00",
  "active" : true,
  "type" : "systemwidealert",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/server/api/system/systemwidealerts/1"
    }
  }
}
```

Status codes:
* 200 OK - if the operation succeeded 
* 404 Unauthorized - if the requested alert is inactive and the user is not an admin


## Retrieve all alerts
**GET /api/system/systemwidealerts**
This endpoint returns a list of all system-wide alerts.
Inactive alerts are included

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user


## Create a new alert
**POST /api/system/systemwidealerts**
This endpoint allows administrators to create a new system-wide alert.

Example request:
```json
{
  "message": "Alert message to show in the banner",
  "allowSessions": "admin",
  "countdownTo": "2023-04-05T00:00:00.000Z",
  "active": true
}
```

The value of `countdownTo` must be a UTC datetime string.

The possible values for `allowSessions` are
* "all": allow all sessions
* "current": allow current sessions only (admins will still be allowed to log in)
* "admin": allow admin sessions only (other users will be logged out)

Status codes:
* 201 Created - if the operation succeeded
* 400 Bad request - if a system-wide alert already exists in the database
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user
* 422 Unprocessable Entity - if the request body cannot be parsed


## Modify an existing alert
**PUT /api/system/systemwidealerts/<:alert-id>**
This endpoint allows administrators to edit an existing system-wide alert.

Example request:
```json
{
  "message": "Another message",
  "allowSessions": "all",
  "countdownTo": "2024-05-06T00:00:00.000Z",
  "active": true
}
```

The value of `countdownTo` must be a UTC datetime string.

The possible values for `allowSessions` are
* "all": allow all sessions
* "current": allow current sessions only (admins will still be allowed to log in)
* "admin": allow admin sessions only (other users will be logged out)

Status codes:
* 200 OK - if the operation succeeded
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user
* 404 Not found - if no alert exists for the requested identifier
* 422 Unprocessable entity - if the request body cannot be parsed
