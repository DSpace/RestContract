# System Config Variables

[Back to the list of all defined endpoints](endpoints.md)

This endpoint exposes the general repository configuration variables that administrators may reference inside email templates (e.g. `${config.get('dspace.name')}`).

It returns the subset of configuration properties allowed in templates, as defined by repository configuration (e.g. `message.templates.allowed-config`).

Only administrators are allowed to access these variables. They are exposed here — decoupled from the [/api/system/emailtemplates](emailtemplates.md) payloads — so clients fetch the identical list once instead of receiving it duplicated inside every template.

---

## Retrieve all system config variables

GET `/api/system/systemconfigvariables`

Returns a list of all allowed system configuration variables, ordered alphabetically by key.

### Example Response (paginated, HAL-embedded)

```json
{
  "_embedded": {
    "systemconfigvariables": [
      {
        "key": "dspace.name",
        "value": "DSpace at My University",
        "placeholder": "${config.get('dspace.name')}",
        "type": "systemconfigvariable",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/system/systemconfigvariables/dspace.name"
          }
        }
      },
      {
        "key": "dspace.ui.url",
        "value": "http://localhost:4000",
        "placeholder": "${config.get('dspace.ui.url')}",
        "type": "systemconfigvariable",
        "_links": {
          "self": {
            "href": "http://localhost:8080/server/api/system/systemconfigvariables/dspace.ui.url"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/system/systemconfigvariables"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

### Variable Properties

* **key**: Configuration property key (e.g. `dspace.name`). Pattern `^[a-zA-Z0-9._-]+$`, max 255 chars.
* **value**: Resolved value of the configuration property, max 2000 chars.
* **placeholder**: Velocity expression to insert into a template (e.g. `${config.get('dspace.name')}`), max 255 chars.

### Status Codes for Retrieve All

* 200 OK - if the operation succeeded
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user

---

## Retrieve a specific system config variable

GET `/api/system/systemconfigvariables/<:property-key>`

Returns a single configuration variable by its property key (e.g., `dspace.name`).

### Example Response

```json
{
  "key": "dspace.name",
  "value": "DSpace at My University",
  "placeholder": "${config.get('dspace.name')}",
  "type": "systemconfigvariable",
  "_links": {
    "self": {
      "href": "http://localhost:8080/server/api/system/systemconfigvariables/dspace.name"
    }
  }
}
```

### Status Codes for Retrieve Specific

* 200 OK - if the operation succeeded
* 401 Unauthorized - if not logged in
* 403 Forbidden - if logged in as a non-admin user
* 404 Not Found - if no allowed variable exists with the specified key
