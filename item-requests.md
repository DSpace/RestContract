# Item Request Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This supports the Request a Copy feature.

## Main Endpoint
**/api/tools/itemrequests**

Provide access to requests. It returns the list of existing requests avoiding to expose token to not admin users.  **NOT IMPLEMENTED**

Example: to be provided

## Single Request
**/api/tools/itemrequests/<:token>**

Provide detailed information about a specific request. The JSON response document will resemble this:
```json
{
  "id":1,
  "decisionDate":null,
  "expires":null,
  "requestDate":"2021-09-17T14:55:50.089+00:00",
  "token":"c19e820ea0b270f0ec98323864dcc8b8",
  "acceptRequest":false,
  "allfiles":false,
  "type":"itemrequest",
  "bitstreamId":"dca2dadb-7028-40a9-ab39-1a47726df7ef",
  "itemId":"71d9fb0c-cc36-41c1-b1d3-63887b414fca",
  "requestEmail":"jsmith@example.com",
  "requestMessage":"Please send me a copy of this.",
  "requestName":"John Smith",
  "_links":{
    "bitstream":{
      "href":"https://api7.dspace.org/server/api/tools/itemrequests/1/bitstream"
    },
    "item":{
      "href":"https://api7.dspace.org/server/api/tools/itemrequests/1/item"
    },
    "self":{
      "href":"https://api7.dspace.org/server/api/tools/itemrequests/1"
    }
  }
}
```

Item properties:

  * type: always "itemrequest".  READ-ONLY
  * id: internal unique identifier of the request.  READ-ONLY
  * itemId: UUID of the requested Item.
  * bitstreamId: UUID of the requested bitstream.
  * allfiles: true if the request is for all bitstreams of the item.
  * requestEmail: email address of the person requesting the files.
  * requestName: Human-readable name of the person requesting the files.
  * requestMessage: arbitrary message provided by the person requesting the files.
  * requestDate: date that the request was recorded.  READ-ONLY.
  * expires: date on which the request is considered expired.
  * acceptRequest: true if the request has been granted.
  * decisionDate: date that the request was granted or denied.  READ-ONLY.
  * token: opaque string which uniquely identifies this request.  READ-ONLY

Exposed links:

  * item: the item requested
  * bitstream: the bitstream requested

Return codes:

* 200 OK - if the operation succeeded
* 404 NOT FOUND - if the token is unknown (no such request exists)

## Creating a Request
**POST /api/tools/itemrequests**

Anyone may create an item request.  The Content-Type is JSON.  Example:
```json
{
    "itemId": "71d9fb0c-cc36-41c1-b1d3-63887b414fca",
    "allfiles": false,
    "bitstreamId": "dca2dadb-7028-40a9-ab39-1a47726df7ef",
    "requestEmail": "jqpublic@example.com",
    "requestName": "John Q. Public",
    "requestMessage": "Please send this to me."
}
```
`bitstreamId` is ignored and may be omitted if `allfiles` is `false`.  `requestMessage` is optional.  `requestEmail` and `requestName` are ignored and may be omitted if the session is authenticated -- these fields will be filled from the session user's EPerson.  If the session is anonymous then `requestEmail` is required.  `bitstreamId` is required if `allfiles` is false.  `itemId` is always required.

The response is empty, generated token should not be exposed.

An appropriate person will be notified that the request has been filed.

Return codes:

* 201 CREATED - if the operation succeeded
* 401 UNAUTHORIZED - if anonymous requests are disabled and the session is unauthenticated
* 422 UNPROCESSABLE ENTITY - if the POSTed document could not be interpreted, the Item or Bitstream could not be found, or required fields are missing

## Accepting / Denying a Request
**PUT /api/tools/itemrequests/<:token>**

Anyone may accept or deny a request.  Access is controlled by keeping the token confidential.  The Content-Type is JSON.  Example:
```json
{
    "acceptRequest": true,
    "subject": "Request copy of document",
    "responseMessage": "Approved.  Documents attached.",
    "suggestOpenAccess": "false"
}
```
`acceptRequest` is required to set the status of a request.  `responseMessage`, `subject` and `suggestOpenAccess` are not part of the request and are not stored.  Other fields will be ignored -- requests are not updatable.

Additional Fields:

* responseMessage:  included in the body of the message sent to the requester.  OPTIONAL.
* subject:  a subject line for the response message.  OPTIONAL.
* suggestOpenAccess:  if `true`, trigger a message to repository administrators suggesting that access to the requested object(s) be opened.  OPTIONAL.

Return codes:

* 200 OK - if the operation succeeded
* 401 UNAUTHORIZED - if the current session is anonymous
* 403 FORBIDDEN - if the current session is not authenticated to one of the configured approvers
* 422 UNPROCESSABLE ENTITY - if the request could not be found or `acceptRequest` was not specified.

## Linked entities
### item entries
**/api/tools/itemrequests/<:id>/item **
### bitstream entries
**/api/tools/itemrequests/<:id>/bitstream **
