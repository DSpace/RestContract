## Main Endpoint
**GET /api/integration/qasources**

Provide access to the Quality Assurance sources. It returns the list of the Quality Assurance sources.

```json
[

  {
    id: "openaire",
    type: "qasource",
    totalEvents: "33"
  },
  {
    id: "another-source",
    type: "qasource",
    lastEvent: "2020/10/09 10:11 UTC",
    totalEvents: "21"
  },
  ...
]
```
Attributes:
* lastEvent: the date of the last update from the specific Quality Assurance source
* totalEvents: the total number of quality assurance events provided by the Quality Assurance source
* id: is the identifier to use in GET Single Source

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access

## GET Single Source
**GET /api/integration/qasources/<:qasource-id>**
​
Provide detailed information about a specific Quality Assurance source. The JSON response document is as follow
​
```json
{
  id: "openaire",
  type: "qasource",
  totalEvents: "33"
}
 ```
​
Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access
* 404 Not found - if the source doesn't exist
