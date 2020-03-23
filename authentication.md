# Authentication
[Back to the list of all defined endpoints](endpoints.md)

The authentication mechanism since DSpace 7 is based on JSON Web Token
Information about the underline implementation are [available on the wiki](https://wiki.duraspace.org/display/DSPACE/REST+Authentication)

## Login
**POST /api/authn/login**

This endpoint only accept the POST method. Parameters and body structure depend on the authentication method to use.

A WWW-Authenticate header is returned listing the different authentication method supported by the system.
Below an example listing the password and shibboleth authentication:  
`WWW-Authenticate: shibboleth realm="DSpace REST API", location="https://dspace7.4science.cloud/Shibboleth.sso/Login?target=https%3A%2F%2Fdspace7.4science.cloud", password realm="DSpace REST API"`

Return codes
- 200 Ok. If the authentication succeed. The JWT will be returned in the response Header Authorization. 
- 401 Unauthorized. If the login fails. The response Header WWW-Authentication must be inspected to discover the supported authentication method

### Username Password based authentication
Parameters must be sent in the body of a x-www-form-urlencoded request, i.e

```
curl -v -X POST https://{dspace-server.url}/server/api/authn/login --data "user=dspacedemo%2Badmin%40gmail.com&password=dspace"
```

Please note that curl assume `-H 'Content-Type: application/x-www-form-urlencoded'` for POST request as default.

This call will return a JWT (JSON Web Token) in the response in the Authorization header according to the bearer scheme. This token has to be used in subsequent calls to provide your authentication details. For example:

```
curl -v "https://{dspace-server.url}/api/core/items" -H "Authorization: Bearer eyJhbG...COdbo"
```

### Shibboleth authentication
The shibboleth realm exposes a location attribute that must be followed by the client to establish a shibboleth session so that subsequent call to the login endpoint will succeed returning the JWT.

### Refresh authentication
Tokens are only valid for a configurable amount of time (see below). When a token is about to expire (the timestamp provided in the exp claim, see below), you can request a new token with a new expiration time (by default 30 minutes). To do so send the token using the Header `Authorization: Bearer ...` to the login endpoint without other parameters. As a response you'll get a new freshly issued token (again in the Authorization header of the response).

i.e

```
curl -v "http://{dspace-server.url}/api/authn/login" -H "Authorization: Bearer eyJhbG...COdbo"
```

Which will return something like this:

```
HTTP/1.1 200 OK
Authorization: Bearer edoDfG...S0df
```

### JSON Web Token structure
The JWT contains the following claims

claim | description
--- | ---
eid|Contains the id of the eperson
sg | Contains the id's of the special groups to which a user belongs
exp | Contains the expiration date when a token will expire

## Logout
** /api/authn/logout **

To logout and invalidate the JWT token, send the token in the Authorization header with the bearer scheme to the endpoint either with a GET or POST request

```
curl -v "http://{dspace-server.url}/api/authn/logout" -H "Authorization: Bearer eyJhbG...COdbo"
```

This invalidate the token on the server side with the result to log the user out on every device or browser.

Return code
- 204 No content

Invalid or missing token are not reported, i.e. the endpoint will always return 204 also if no token is supplied or the token is invalid

## Status
** /api/authn/status **

The authentication status can be checked by sending your received token to the status endpoint in the Authorization header in a GET request:

```
curl -v "http://{spring-rest.url}/api/authn/status" -H "Authorization: Bearer eyJhbG...COdbo"
```

This will return the authentication status, E.G.:

```json
{
  "okay" : true,
  "authenticated" : true,
  "type" : "status",
  "_links" : {
    "eperson" : {
      "href" : "http://${dspace-server.url}/api/eperson/epersons/2245f2c5-1bed-414b-a313-3fd2d2ec89d6"
    }
  },
  "_embedded" : {
    "eperson" : {
      "uuid" : "2245f2c5-1bed-414b-a313-3fd2d2ec89d6",
      "email" : "test@dspace.com",
      ...
      }
    }
  }
}
```

Fields
- Okay: True if REST API is up and running, should never return false
- Authenticated: True if the token is valid, false if there was no token or the token wasn't valid
- Type: Type of the endpoint, "status" in this case

Links	
- returns a link to the authenticated eperson

Embedded
- Embeds the authenticated eperson

Return code
- 200 Ok in all the scenario both authenticated than not authenticated (valid token, invalid token or missing token)

## Log in as

For any request, an `X-On-Behalf-Of` header can be included.
If the user is authorized to use this header (the user is an admin and login as is allowed), the request will be processed using the account of the provided user.  
Verifying whether the user is authorized to use this header can happen using the "loginOnBehalfOf" [feature](features.md), verified against the site

Sample request: 
```
curl -v "http://{dspace-server.url}/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb" -H "Authorization: Bearer eyJhbG...COdbo" -H "X-On-Behalf-Of: 028dcbb8-0da2-4122-a0ea-254be49ca107"
```

The Authorization header remains the same, still linked to the actual admin using the `X-On-Behalf-Of` header.

Status codes:
* 400 Bad Request - if the X-On-Behalf-Of header doesn't contain a valid EPerson UUID
* 403 Forbidden - if you are not authorized to act on behalf of the given user
* Any status code of the functionality being used
