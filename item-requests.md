# Item Request Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This supports the Request a Copy feature.

## Main Endpoint
**/api/tools/copyRequests**

Provide access to requests. It returns the list of existing requests.

Example: to be provided

## Single Request
**/api/tools/copyRequests/<:id>**

Provide detailed information about a specific request. The JSON response document will resemble this:
```json
{
  "id": "12ef",
  "token": "56cd",
  "allfiles": true,
  "request_email": "jqpublic@example.com",
  "request_name": "John Q. Public",
  "request_date": "20180718T205000",
  "accept_request": false,
  "decision_date": "20180718T295100",
  "expires": "20181231T235959",
  "request_message": "Please send this to me.",
  "item_id": "36ab",
  "bitstream_id": "44cd"
}
```

Exposed links:

  * item: the item requested
  * bitstream: the bitstream requested

## Linked entities
### item entries
**/api/tools/copyRequests/<:id>/item **
### bitstream entries
**/api/tools/copyRequests/<:id>/bitstream **
