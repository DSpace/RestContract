# Captcha Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/captcha**   

Not allowed. At this time, only the `challenge` endpoint is available.

## Retrieve a challenge from the (ALTCHA) captcha service
**/api/captcha/challenge**

If the captcha provider is enabled and configured to be 'altcha', this endpoint will return a challenge for the browser to complete as proof-of-work.

This is not an addressable HATEOAS object, or endpoint, but rather a JSON object that is meant to be consumed and processed in the browser.

The JSON body will contain the algorithm, salt, signature, and challenge. An example is below:

```json
{
  "algorithm": "SHA-25", 
  "salt": "dcf5eba26e",
  "challenge": "0d8dd34089fdd610bd9a8857ea1fa4a5f9fe4b53f5df0c4e1eff6dc987c4d2bf",
  "signature": "dfe4ec56f3d61e3a021b1c3b3ea4c7d6aea9812ab719ffe130fd386ce0b4158c"
}
```

An example curl call:
```
curl -i https://demo.dspace.org/server/api/captcha/challenge
```

For more information about ALTCHA challenge creation, see: https://altcha.org/api/operations/createchallenge/

The final result of the work is not submitted to this endpoint, but instead included with form data submitted in the captcha-protected form, for final verification and validation.

This endpoint does not relate to Google ReCaptcha, which calls 3rd party services directly from the frontend for captcha validation)

Return codes:
200 OK - if the operation succeeded and the JSON body is returned
400 Bad Request - if the captcha provider is not enabled or configured to be 'altcha', or the algorithm is not supported
500 Internal Server Error - if the challenge or hmac hash cannot be calculated