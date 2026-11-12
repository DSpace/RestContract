# Email Templates

[Back to the list of all defined endpoints](endpoints.md)

This endpoint exposes DSpace email templates to administrators, allowing them to view and edit email templates stored in `{dspace.dir}/config/emails/`.

Only administrators are allowed to access and edit these templates.
Templates cannot be created or deleted via the REST API because they correspond to hardcoded templates in the DSpace backend.

System configuration variables usable inside templates (e.g. `${config.get('dspace.name')}`) are deliberately **not** embedded in template payloads. They are exposed via the dedicated [/api/system/systemconfigvariables](systemconfigvariables.md) endpoint.

## Optimistic Concurrency Control

To prevent concurrent administrators from overwriting each other's changes, this endpoint implements optimistic concurrency control via HTTP conditional headers:

* **ETag Header**: Every `GET` and successful `PUT` response includes an `ETag` response header containing a SHA-256 hash of the template content (e.g., `ETag: "da56428b0c..."`).
* **If-None-Match**: When retrieving a template with `GET`, clients may provide the `If-None-Match` header containing an entity tag, a comma-separated list of entity tags, weak validators (e.g. `W/"..."`), or `*`. If any candidate matches the current template, the server responds with **304 Not Modified**.
* **If-Match Required on PUT**: Every `PUT` request **must** provide the `If-Match` request header. It accepts an entity tag, a comma-separated list of entity tags, weak validators (e.g. `W/"..."`), or `*` for unconditional overwrite.
  * If `If-Match` is missing, the server responds with **428 Precondition Required**.
  * If none of the provided ETags match the current template version, the server responds with **412 Precondition Failed**.

---

## Retrieve all email templates

GET `/api/system/emailtemplates`

Returns a list of all email templates, ordered alphabetically by name.

### Status Codes for Retrieve All

* 200 OK - if the operation succeeded
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user

---

## Retrieve a specific email template

GET `/api/system/emailtemplates/<:template-name>`

Returns a single email template by its filename identifier (e.g., `register`, `feedback`).

### Example Response

```json
{
  "name": "register",
  "content": "## E-mail sent to DSpace users when they register for an account\n##\n## Parameters: {0} is expanded to a special registration URL\n##\n#set($subject = \"${config.get('dspace.name')} Account Registration\")\nTo complete registration, click: ${params[0]}\n",
  "subject": "${config.get('dspace.name')} Account Registration",
  "mimetype": "text/plain",
  "variables": [
    {
      "index": 0,
      "description": "is expanded to a special registration URL",
      "placeholder": "${params[0]}"
    }
  ],
  "lastModified": "2026-09-05T12:00:00Z",
  "etag": "da56428b0c502de5a058cdd47cdf5fc446d90fc68ed2cf096d3bf6ae53ba568a",
  "type": "emailtemplate",
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/system/emailtemplates/register"
    }
  }
}
```

### Template Properties

* **name**: Unique template identifier matching filename. Pattern `^[a-zA-Z0-9._-]+$`, max 255 chars.
* **subject**: Extracted subject line from `#set($subject = ...)` directive, max 500 chars.
* **mimetype**: Enum indicating the content type. Possible values:
  * `"text/plain"`: Standard plain text email (default).
  * `"text/html"`: HTML-formatted email, detected when the template specifies `#set($mimetype = "text/html")` (supported via [DSpace PR #11755](https://github.com/DSpace/DSpace/pull/11755)).
* **content**: Raw VTL template body, 10 to 50,000 chars.
* **variables**: Array of parsed parameter variables (`index`, `description` max 500 chars, `placeholder` max 30 chars e.g. `${params[0]}`).
* **lastModified**: ISO-8601 timestamp of when the file was last modified on disk.
* **etag**: Deterministic SHA-256 hash of the template content for concurrency control.

### Status Codes for Retrieve Specific

* 200 OK - if the operation succeeded
* 304 Not Modified - if any candidate in `If-None-Match` matches current ETag (or `*`)
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user
* 404 Not Found - if no template exists with the specified name

---

## Update an email template

PUT `/api/system/emailtemplates/<:template-name>`

Replaces the full content of the email template. The submitted content is preserved as-is (retaining leading indentation and blank lines) while ensuring a trailing newline. The template content is validated against Velocity (VTL) syntax rules, XSS sanitization (OWASP), and size constraints (10 to 50,000 characters).

### Required Request Headers

* `Content-Type: application/json`
* `If-Match: "<current-etag>"` (also accepts a comma-separated list of ETags, `W/"<current-etag>"`, or `*`)

### Example Request Body

```json
{
  "content": "## E-mail sent to DSpace users when they register for an account\n##\n## Parameters: {0} is expanded to a special registration URL\n##\n#set($subject = \"${config.get('dspace.name')} Account Registration\")\nTo complete registration, please visit: ${params[0]}\n"
}
```

### Status Codes for Update

* 200 OK - if the template was successfully updated (returns updated template and new `ETag` header)
* 400 Bad Request - if the content fails validation (invalid VTL syntax, XSS script tags, or length < 10 or > 50,000 chars)
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user
* 404 Not Found - if no template exists with the specified name
* 412 Precondition Failed - if none of the candidate ETags in `If-Match` match the current template version
* 428 Precondition Required - if the `If-Match` header was omitted
