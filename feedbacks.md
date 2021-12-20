# Feedbacks Endpoints
[Back to the list of all defined endpoints](endpoints.md)

The /api/tools/feedbacks endpoints are used to send feedback (via email) to DSpace's configured 'feedback.recipient'.
At this time submitted feedbacks are not visible or accessible via the REST API.

## Main Endpoint
**/api/tools/feedbacks**   

As we don't have yet an use case to iterate over all the existent feedbacks the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single Feedback
**/api/tools/feedbacks/<:id>**

As we don't have yet an use case to get a single feedback the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Creating a feedback
**POST /api/tools/feedbacks**

Request body:
```json
{
  "e-mail": "myemail@test.com",
  "message": "DSpace is the best!",
  "page": "/home"
}
```

The json body must be valid that mean
- e-mail must be provided
- message must be provided

Return codes:
* 201 Created - if the operation succeed
* 400 Bad Request - if some of the fields e-mail or message is missing
* 404 Not found - if the property 'feedback.recipient' is not configured
