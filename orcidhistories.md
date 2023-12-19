# ORCID History Endpoints
[Back to the list of all defined endpoints](endpoints.md)

These endpoints allow you to manipulate the Orcid History records associated with a specific profile.
Only the EPerson who is the owner of the Profile Item can access them.

An Orcid History records is created starting from the processing of an Orcid Queue record,
an operation that involves an attempt to synchronize the entity with the ORCID registry.

These endpoints are only available when orcid.sychronization-enabled=true.

## Single ORCID History entry
**GET /api/eperson/orcidhistories/<:id>**

Provide detailed information about a specific ORCID history entry. The JSON response document is as follow
```json
{
  "id": 20,
  "profileItemId": "87d24f06-0ca5-4abe-84f9-bfd36a8236d8",
  "entityId": "66e0d773-ddeb-4b6b-bcac-ced2ae67141c",
  "status": "201",
  "putCode": "123456",
  "responseMessage": "<xml><work>...</work>",
  "timestamp": "2014-01-01T23:28:56.782Z", 
  "type": "orcidhistory",
  "_links": {
    "self": {
      "href": "**/api/eperson/orcidhistories/20"
    }
  }
}
```

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions (only the owner of the profile is allowed)
* 404 Not found - if an ORCID queue entry with the given id doesn't exist

The `profileItemId` attribute represent the id of the researcher profile item that is associated with the the entity item with id `entityId`; the entity is the item that should be sent to the ORCID register. The `putCode` attribute, if presente, represent the unique code that ORCID registry associate to the stored entity. The `status` attribute is the response status of the related call to ORCID to send the entity. The `responseMessage` is the complete response body coming from the ORCID api when the call to send the entity is made. The `lastAttempt` and the `sucessAttempt` are two timestamps that represent respectively the last attempt to send the entity info to ORCID and the last moment in which that send was successful.

## Send to ORCID registry
**POST /api/eperson/orcidhistories**

To send an ORCID queue entry to the ORCID api and create a record into the ORCID history a reference of an ORCID queue record must be posted to the ORCID history resource endpoint (/api/eperson/orcidhistories). The ORCID queue record must be supplied as URI in the request body using the text/uri-list content-type.

An optional query param named `forceAddition` with value true or false could be provided to force the send of a new resource to the ORCID api without an update of an existing resource even if for the provided ORCID queue record there is a putCode.

Return codes:
* 201 OK - if the operation succeed and the orcid history record is created
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions (only the owner of the profile is allowed)
* 404 Not found - if no ORCID queue entry exists related to the fiven URI
* 422 Unprocessable entity - if the ORCID object to synchronize with ORCID registry is not valid

An example curl call:
```
 curl -i -X POST https://demo.dspace.org/server/api/eperson/orcidhistories \
 -H "Content-Type:text/uri-list" \
 --data "https://demo.dspace.org/server/api/eperson/orcidqueues/10"
```
