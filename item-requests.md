# Item Request Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This supports the Request a Copy feature.

## Main Endpoint
**/api/tools/itemrequests**

Provide access to requests. It returns the list of existing requests.  **NOT IMPLEMENTED**

Example: to be provided

## Single Request
**/api/tools/itemrequests/<:id>**

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
      "href":"http://localhost/api/tools/itemrequests/1/bitstream"
    },
    "item":{
      "href":"http://localhost/api/tools/itemrequests/1/item"
    },
    "self":{
      "href":"http://localhost/api/tools/itemrequests/1"
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
The response contains the complete request in JSON format, as shown in Single Request.

An appropriate person will be notified that the request has been filed.

## Accepting / Denying a Request
**PUT /api/tools/itemrequests/<:token>**

Anyone may accept or deny a request.  Access is controlled by keeping the token confidential.  The Content-Type is JSON.  Example:
```json
{
    "acceptRequest": true,
    "responseMessage": "Approved.  Documents attached."
}
```
"acceptRequest" is required to set the status of a request.  "responseMessage" is optional.  "responseMessage" is not a part of the request and is not stored.  Other fields will be ignored -- requests are not updatable.

## Linked entities
### item entries
**/api/tools/itemrequests/<:id>/item **
### bitstream entries
**/api/tools/itemrequests/<:id>/bitstream **
