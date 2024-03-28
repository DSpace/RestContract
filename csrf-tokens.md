# CSRF Protection
[REST Overview Documentation](README.md)

All modifying requests (`POST`, `PUT`, `PATCH`, `DELETE`, etc) **require** that the client send a valid CSRF token
in the `X-XSRF-TOKEN` header, otherwise a `403 Forbidden` error will occur. 

## How to obtain/reset a CSRF token?

The CSRF token is sent automatically to the client in a prior request (often a `GET`) and may be changed during your session at any time. The client MUST watch for changes to the CSRF token in every request (although in practice the token should primarily only change during login/logout or authentication refresh, etc)
See [How does CSRF protection work in DSpace REST API?](#how-does-csrf-protection-work-in-dspace-rest-api) below for more details.

A `GET` endpoint is also available to force a CSRF token refresh. See below.

### GET /api/security/csrf

Provides a new CSRF Token to the client for usage in later requests. This endpoint SHOULD BE USED SPARINGLY as every call to this endpoint will create a new CSRF token. The DSpace REST API automatically manages CSRF tokens and sends them back to the client _only when the token needs to be changed_ (e.g. on login/logout, etc). Using this endpoint to refresh CSRF tokens frequently may result in unexpected behaviors or even performance issues. The primary purpose of this endpoint is to obtain the *first* CSRF token (if not yet sent by the REST API).

This endpoint returns an empty response with the newly generated CSRF Token in the `DSPACE-XSRF-TOKEN` HTTP Header. It will also save this CSRF Token to the `DSPACE-XSRF-COOKIE` server-side Cookie, for later verification. See [How does CSRF protection work in DSpace REST API?](#how-does-csrf-protection-work-in-dspace-rest-api) below for more details about this header and cookie.

Error codes:
* 204 No Content - if the operation succeeded no body will be returned. The new token will be returned in the CSRF header and cookie.
* 403 Forbidden - This endpoint will only respond to GET requests. All other requests types (POST/PUT) will return a 403.

## What is CSRF and why is this necessary?

CSRF (or sometimes XSRF) refers to [Cross Site Request Forgery](https://owasp.org/www-community/attacks/csrf) attacks, where
an end user may be tricked into executing unwanted actions on a web application in which they are currently authenticated.

DSpace follows [OWASP guidelines to CSRF prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
by enabling [Spring Security's CSRF Protection](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#csrf)

DSpace uses a customized version of [Spring Security's CSRF Protection](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#csrf)
which also works cross-domain (allowing clients to be on entirely separate domains from the REST API).
Please keep in mind that clients on other domains *must* still be trusted by the REST API by configuring the [CORS (Cross-Origin Resource Sharing)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
`Access-Control-Allow-Origin` header. This is configurable via the `rest.cors.allowed-origins` configuration in the DSpace Backend.

## How does CSRF protection work in DSpace REST API?

The DSpace REST API (backend) uses [Spring Security's built-in CSRF Protection](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html).  Specifically it uses a customized version of Spring Security's `CookieCsrfTokenRepository` to store the CSRF token in a cookie between requests as described below.

Here's how it works:

1. The DSpace REST API generates a CSRF Token (via Spring Security), storing it in a `HttpOnly` Cookie named `DSPACE-XSRF-COOKIE`, and sending
it back to the client in an HTTP header named `DSPACE-XSRF-TOKEN`.
   * This token is often generated on your first request to the REST API, but may also be updated at any time.
   * If your REST API is running via HTTP, then the `DSPACE-XSRF-COOKIE` Cookie will be created with `SameSite=Lax`. This setting means that the cookie will not be sent (by your browser) to any other domains. Effectively, this will block all logins from any domain that is not the same as the REST API. Therefore, we only recommend HTTP in development environments.
   * If your REST API is running via HTTPS, then the `DSPACE-XSRF-COOKIE` Cookie will be created with `SameSite=None; Secure`. This setting means the cookie will be sent cross domain, but only for HTTPS requests. This is recommended for all Production environments.
2. The client **MUST** store/keep a copy of this CSRF token (usually by watching for the `DSPACE-XSRF-TOKEN` header in every response),
and update that stored copy whenever a new token is sent.
   * For example, the [DSpace Angular UI](https://github.com/DSpace/dspace-angular) stores watches for this token from the `DSPACE-XSRF-TOKEN` header, and stores it in a client-side cookie named `XSRF-TOKEN` (as this is the default cookie name that Angular apps use for reading a CSRF token).  More information on how this works can be found below.
3. Whenever the client wishes to make a modifying request (`POST`, `PUT`, `PATCH`, `DELETE`, etc), 
the current CSRF token **MUST** be sent back in the `X-XSRF-TOKEN` header of the request.
   * Keep in mind that [authentication](authentication.md) (via `/api/authn/login`) requires `POST` and therefore also requires a valid CSRF token.
   * NOTE: non-modifying requests (`GET`, `HEAD`, `OPTIONS`, etc) do not require the token.
4. During a modifying request, the REST API will compare the value of the CSRF token in the request's
`X-XSRF-TOKEN` header to the value in the `DSPACE-XSRF-COOKIE` Cookie. If the values match, the request proceeds.
If the values do not match, a `403 Forbidden` error is returned.
   * Keep in mind that if the `DSPACE-XSRF-COOKIE` Cookie was blocked (or not sent back to the REST API) during this process, the same error will occur. So, you must ensure this Cookie is sent back (when using a browser this is usually done automatically, provided the `SameSite` settings allow it)

## How does CSRF protection work in DSpace User Interface?

The DSpace User Interface includes some minor customizations to the default [CSRF protection built into Angular](https://angular.io/guide/http-security-xsrf-protection) based on the behavior of the backend.

Here's how it works:
1. The User Interface submits a request to the REST API which generates a CSRF Token (as described above).
2. The REST API sends the generated CSRF Token to the User Interface in an HTTP header named `DSPACE-XSRF-TOKEN`.
    * NOTE: The User Interface is unable to "see" the `DSPACE-XSRF-COOKIE` cookie where the REST API also stores this token.  This cookie has `HttpOnly` which makes it unreadable to Javascript. In addition, the User Interface and REST API may be on separate domains (which will often results in the web browser blocking the cross-domain cookie).
3. The User Interface reads the CSRF Token from the `DSPACE-XSRF-TOKEN` header and **stores it** in a client-side cookie named `XSRF-TOKEN` (as this is the default cookie name that Angular apps use for reading a CSRF token).
4. When the User Interface nexts sends a modifying request (`POST`, `PUT`, `PATCH`, `DELETE`, etc), it *reads* the current CSRF Token from the client-side `XSRF-TOKEN` cookie and then **copies it** into the `X-XSRF-TOKEN` HTTP Header to send back to the REST API.
5. On each later response from the REST API, the User Interface watches for changes to the CSRF Token by checking for the `DSPACE-XSRF-TOKEN` header.  If changes are found, it copies the new token into the client-side cookie named `XSRF-TOKEN` for future requests.

