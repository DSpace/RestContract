## Main Endpoint
**GET /api/integration/nbsources**

Provide access to the Notification sources. It returns the list of the Notification sources.

```json
[

  {
    id: "openaire",
    type: "nbsource",
    totalEvents: "33"
  },
  {
    id: "another-source",
    type: "nbsource",
    lastEvent: "2020/10/09 10:11 UTC",
    totalEvents: "21"
  },
  ...
]
```
Attributes:
* lastEvent: the date of the last update from the specific Notification source
* totalEvents: the total number of suggestions provided by the Notification source
* id: is the identifier to use in GET Single Source

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access

## GET Single Source
**GET /api/integration/nbsources/<:nbsource-id>**
​
Provide detailed information about a specific Notification source. The JSON response document is as follow
​
```json
{
  id: "openaire",
  type: "nbsource",
  totalEvents: "33"
}
 ```
​
Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access
* 404 Not found - if the source doesn't exist