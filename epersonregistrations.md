# EPerson registration Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/registrations**

As we don't have a use case to iterate over all the existent registrations, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Single EPerson registration

As we don't have a use case to retrieve an eperson registration based on the email, the single endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Search EPerson registration
**/api/eperson/registrations/search/findByToken?token=<:token>**

Exposes the registered email address based on the token or the `netId`.
Also exposes whether it's a new user registration, or a password reset for an existing user. It can expose also `metadata` involved during the registration process, with the overridden values. Lastly, the `registrationType` fields is used to discriminate between the registration flow / procedure.

### Examples
1. User registration not linked to an eperson:
```json
{
  "id": 1,
  "email": "user@institution.edu",
  "user": null,
  "type": "registration"
}
```
2. User registration linked to an eperson:
```json
{
  "id": 2,
  "email": "user@institution.edu",
  "user": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "type": "registration"
}
```
3. User registration done using **orcid** that didn't provide an email:
```json
{
  "id": 3,
  "email": null,
  "user": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "type": "registration",
  "registrationType": "orcid",
  "netId": "0000-1111-2222-3333",
  "registrationMetadata": {
    "eperson.orcid": [
      {
        "value": "0000-1111-2222-3333",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1
      }
    ],
    "eperson.firstname": [
      {
        "value": "Power",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1,
        "overrides": "Power U."
      }
    ],
    "eperson.lastname": [
      {
        "value": "User",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1
      }
    ]
  }
}
```
> Note that the `registrationMetadata` is filled with metadata coming from the external login provider (i.e. **ORCID**) and enriched with the `overrides` field containg metadata value coming from the linked `user`.
> 
> In the previous case the `eperson.firstname` metadata related to the `registration-data` **overrides** the value of the `metadata` previously setted for that related `user` (i.e. `Power U.`).

4. User registration done using **orcid** that provided an email:
```json
{
  "id": 4,
  "email": "power-user@orcid.org",
  "user": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "type": "registration",
  "registrationType": "orcid",
  "netId": "0000-1111-2222-3333",
  "registrationMetadata": {
    "email": [
      {
        "value": "power-user@orcid.org",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1,
        "overrides": "power-user@dspace.org"
      }
    ],
    "eperson.orcid": [
      {
        "value": "0000-1111-2222-3333",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1
      }
    ],
    "eperson.firstname": [
      {
        "value": "Power",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1,
        "overrides": "Power U."
      }
    ],
    "eperson.lastname": [
      {
        "value": "User",
        "language": null,
        "authority": "",
        "confidence": -1,
        "place": -1
      }
    ]
  }
}
```
> In the previous case the registration has been confirmed by providing a valid email, or just because the orcid login provided an email-account. This email is treated as an additional metadata, but in fact it's just duplicating the `email` field to provide the `overrides` fields.

## Updated EPerson registration
**PATCH /api/eperson/registrations/<:id>?token=<:token>**

To update the EPerson registration, perform a patch with the JSON below to the eperson registrations endpoint, by providing the registration `token` and the `id` of the registration. 

1. Replace the existing email inside the registration:
```json
[
  {
    "op": "replace",
    "path": "/email",
    "value": [ "vincenzo.mecca@4science.com" ]
  }
]
```
2. Add an email inside the registration:
```json
[
  {
    "op": "add",
    "path": "/email",
    "value": [ "vincenzo.mecca@4science.com" ]
  }
]
```

The allowed path is the one involving the `email` field, while the operation allowed are:

- `replace` - if a different value was set
- `add` - if none already set

This method, if successful, will renew the `token` and as a side effect it will send a new Email to the provided `email` value.

We can have the following cases:
- `email` related to an account: a validation link for reviewing the information is sent to that email address.
- new `email`: a link to the same authentication method provided is sent to that email address 

Status codes:

* 204 Created - if the operation succeed with a new token generated, and e new mail sent to the `email` provided in the request
* 401 Unauthorized - if registration is disabled the token provided is not valid or absent
* 422 Unprocessable Entity - if the email address was omitted or the operation is not valid

## Create new EPerson registration
**POST /api/eperson/registrations?accountRequestType={requestType_forgot_or_register}**

To create a new EPerson registration, perform a post with the JSON below to the eperson registrations endpoint (without being authenticated).

```json
{
  "email": "user@institution.edu",
  "type": "registration"
}
```

Requires query parameter 'accountRequestType' set with either 'register' or 'forgot' value, depending on the action requested.

No other properties can be set (e.g. the name cannot be defined)
If successful, an email will be sent with a token allowing the user to continue the registration

Verifying whether a new registration can be created can happen using the "epersonRegistration" [feature](features.md), verified against the site

Status codes:
* 201 Created - if the operation succeed
* 400 Bad Request - if e.g. the query param 'accountRequestType' is not present or contains something else than forgot or register
* 401 Unauthorized - if registration is disabled, you are not authorized to create a new registration
* 422 Unprocessable Entity - if the email address was omitted or the e-mail address is in a domain that is not allowed in config `authentication-password.domain.valid`


## Forgot password

The same endpoint as [Create new EPerson registration](#create-new-eperson-registration) is used.

Using the same endpoint ensures it's not possible for a malicious user to identify which email addresses are registered by attempting a registration and verifying whether the account exists
