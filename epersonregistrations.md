# EPerson registration Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/registrations**

As we don't have a use case to iterate over all the existent registrations, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Single EPerson registration

As we don't have a use case to retrieve an eperson registration based on the email, the single endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Search EPerson registration
**/api/eperson/registrations/search/findByToken?token=<:token>**

Exposes the registered email address based on the token.  
Also exposes whether it's a new user registration, or a password reset for an existing user.

```json
{
  "email": "user@institution.edu",
  "user": null,
  "type": "registration"
}
```

```json
{
  "email": "user@institution.edu",
  "user": "028dcbb8-0da2-4122-a0ea-254be49ca107",
  "type": "registration"
}
```

## Create new EPerson registration
**POST /api/eperson/registrations**

To create a new EPerson registration, perform a post with the JSON below to the eperson registrations endpoint (without being authenticated).

```json
{
  "email": "user@institution.edu",
  "type": "registration"
}
```

No other properties can be set (e.g. the name cannot be defined)
If successful, an email will be sent with a token allowing the user to continue the registration

Verifying whether a new registration can be created can happen using the "epersonRegistration" [feature](features.md), verified against the site

Status codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if registration is disabled, you are not authorized to create a new registration
* 422 Unprocessable Entity - if the email address was omitted

## Forgot password

The same endpoint as [Create new EPerson registration](#create-new-eperson-registration) is used.

Using the same endpoint ensures it's not possible for a malicious user to identify which email addresses are registered by attempting a registration and verifying whether the account exists
