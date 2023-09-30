# Authentication
[Back to the list of all defined endpoints](endpoints.md)

The authentication mechanism since DSpace 7 is based on JSON Web Token (JWT / RFC 7519).

Information about the underline implementation are [available on the wiki](https://wiki.duraspace.org/display/DSPACE/REST+Authentication)

## Login
**POST /api/authn/login**

This endpoint only accepts the `POST` method. Parameters and body structure depend on the authentication method to use.

A `WWW-Authenticate` header is returned listing the different authentication method supported by the system.
Below an example listing the password and shibboleth authentication:  
`WWW-Authenticate: shibboleth realm="DSpace REST API", location="https://demo.dspace.org/Shibboleth.sso/Login?target=https%3A%2F%2Fdemo.dspace.org", password realm="DSpace REST API"`

Return codes
- 200 Ok. If the authentication succeed. The JWT will be returned in the response Header Authorization. 
- 401 Unauthorized. If the login fails. The response Header WWW-Authentication must be inspected to discover the supported authentication method

### Username Password based authentication
Parameters must be sent in the body of a `x-www-form-urlencoded` request, i.e

```
curl -v -X POST https://{dspace-server.url}/server/api/authn/login --data "user=dspacedemo%2Badmin%40gmail.com&password=dspace" -H "X-XSRF-TOKEN: {csrf-token}" -b "DSPACE-XSRF-COOKIE={xsrf-cookie}"
```

Please note that authentication requires passing a valid [CSRF token](csrf-tokens.md) header and cookie, previously obtained from the REST API. Also note that `curl` assumes `-H 'Content-Type: application/x-www-form-urlencoded'` for a POST request as default.

This call will return a JWT (JSON Web Token) in the response in the Authorization header according to the [bearer scheme](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1). This token has to be used in subsequent calls to provide your authentication details. For example:

```
curl -v "https://{dspace-server.url}/api/core/items" -H "Authorization: Bearer eyJhbG...COdbo"
```

### Shibboleth authentication
The shibboleth realm exposes a location attribute that must be followed by the client to establish a shibboleth session so that subsequent call to the login endpoint will succeed returning the JWT.

### Refresh authentication
Tokens are only valid for a configurable amount of time (see below). When a token is about to expire (the timestamp provided in the exp claim, see below), you can request a new token with a new expiration time (by default 30 minutes). To do so send the token using the Header `Authorization: Bearer ...` to the login endpoint without other parameters. As a response you'll get a new freshly issued token (again in the Authorization header of the response).

i.e

```
curl -v -X POST "http://{dspace-server.url}/api/authn/login" -H "Authorization: Bearer eyJhbG...COdbo" -H "X-XSRF-TOKEN: {csrf-token}"
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
**POST /api/authn/logout**

This endpoint only accepts the `POST` method.

To logout and invalidate the JWT token, send the token in the `Authorization` header with the bearer scheme.

```
curl -v -X POST "https://{dspace-server.url}/api/authn/logout" -H "Authorization: Bearer eyJhbG...COdbo" -H "X-XSRF-TOKEN: {csrf-token}"
```

This invalidates the token on the server side which will results in logging out the user _on every device or browser_. It can also be called with params **action** and **return**, required by the Shibboleth Single Logout (front channel), with the same behaviour.

As this endpoint requires a POST, it requires passing a valid [CSRF token](csrf-tokens.md).

Return code
- 204 No content.
- 302 Found. If a successful logout occurs, and a logout page URL is configured

Invalid or missing tokens are not reported, i.e. the endpoint will always return 204 also if no token is supplied, or the token is invalid

## Status
**/api/authn/status**

The authentication status can be checked by sending your received token to the status endpoint in the Authorization header in a GET request:

```
curl -v "http://{dspace-server.url}/api/authn/status" -H "Authorization: Bearer eyJhbG...COdbo"
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
      },
    "specialGroups" : {
        "_embedded" : {
           "specialGroups" : [ {
             "id" : "3d07510c-2c33-48f5-8a04-f63bc9a63296",
             "uuid" : "3d07510c-2c33-48f5-8a04-f63bc9a63296",
             "name" : "A Special Group",
             ... } ],
        "page" : {
            "number" : 0,
            "size" : 20,
            "totalPages" : 1,
            "totalElements" : 1
        },
        "_links" : {
            "self" : {
                "href" : "http://${dspace-server.url}/api/authn/status/specialGroups"
            }
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
- specialGroups: return the special groups associated with the current authentication context. Please note that specialGroups can be present also without an authenticated user (i.e. via IPAuthentication)

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


## Request short lived token

**POST /api/authn/shortlivedtokens**

When clicking on a link to download a protected file in the UI no authentication header will be sent along. This endpoint can provide a short lived token (MAX 2 seconds) that the UI can append to file downloads.
 
The token follows the "JSON Web Token structure", same as the login tokens.
  
 ```
 curl -v -X POST https://{dspace-server.url}/api/authn/shortlivedtokens -H "Authorization: Bearer eyJhbG...COdbo" -H "X-XSRF-TOKEN: {csrf-token}"
 ```
 
 ```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJlaWQiOiJjZDgyNGE2MS05NWJlLTRlMTYtYmNjZC01MWZlYTI2NzA3ZDAiLCJzZyI6W10sImV4cCI6MTU5MDQxMzUwNn0.XRK4ldh9l4My45gJzLtcW97hVUpbtM5oAQsxuQ2V37c",
  "_links": {
    "self": {
      "href": "http://${dspace-server.url}/api/authn/shortlivedtokens"
    }             
  }
}
```  

Return codes
- 200 Ok. 
- 401 Unauthorized. If no user is logged in

**GET /api/authn/shortlivedtokens**

_As of 7.5, this option is no longer supported._  Please use POST instead (see above).

### Using short lived token

The request parameter below can be used for nearly every REST endpoint to perform it with authentication but without needing the authentication header:
* **authentication-token**: optional parameter that authenticates a user, a token needs to be requested from the [following endpoint](authentication.md#Request-short-lived-token) 

The following endpoints are not allowed to use the authentication-token:
* **POST /api/authn/login**: refreshing your long-lived authentication header is not allowed using the short lived token
* **POST /api/authn/shortlivedtokens**: creating a new short lived token is not allowed using the short lived token
