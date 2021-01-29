# CSRF Protection
[REST Overview Documentation](README.md)

All modifying requests (`POST`, `PUT`, `PATCH`, `DELETE`, etc) **require** that the client send a valid CSRF token
in the `X-XSRF-TOKEN` header, otherwise a `403 Forbidden` error will occur. 

The CSRF token is sent to the client in a prior request (often a `GET`) and may be changed during your session at any time.

## What is CSRF and why is this necessary?

CSRF (or sometimes XSRF) refers to [Cross Site Request Forgery](https://owasp.org/www-community/attacks/csrf) attacks, where
an end user may be tricked into executing unwanted actions on a web application in which they are currently authenticated.

DSpace follows [OWASP guidelines to CSRF prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
by enabling [Spring Security's CSRF Protection](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#csrf)

DSpace uses a customized version of [Spring Security's CSRF Protection](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#csrf)
which also works cross-domain (allowing clients to be on entirely separate domains from the REST API).
Please keep in mind that clients on other domains *must* still be trusted by the REST API by configuring the [CORS (Cross-Origin Resource Sharing)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
`Access-Control-Allow-Origin` header.

## How does CSRF protection work in DSpace?

1. The DSpace REST API generates a CSRF Token, storing it in a `HttpOnly` Cookie named `DSPACE-XSRF-COOKIE`, and sending
it back to the client in a header named `DSPACE-XSRF-TOKEN`.
   * This token is often generated on your first request to the REST API, but may also
    be updated at any time.
2. The client **MUST** store/keep a copy of this CSRF token (usually by watching for the `DSPACE-XSRF-TOKEN` header in every response),
and update that stored copy whenever a new token is sent.
3. Whenever the client wishes to make a modifying request (`POST`, `PUT`, `PATCH`, `DELETE`, etc), 
the current CSRF token **MUST** be sent back in the `X-XSRF-TOKEN` header of the request.
   * Keep in mind that [authentication](authentication.md) (via `/api/authn/login`) requires `POST` and therefore also requires a valid CSRF token.
   * NOTE: non-modifying requests (`GET`, `HEAD`, `OPTIONS`, etc) do not require the token.
4. During a modifying request, the REST API will compare the value of the CSRF token in the request's
`X-XSRF-TOKEN` header to the value in the `DSPACE-XSRF-COOKIE` Cookie. If the values match, the request proceeds.
If the values do not match, a `403 Forbidden` error is returned.
