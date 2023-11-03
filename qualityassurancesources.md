## Main Endpoint
**GET /api/integration/qualityassurancesources**

Provide access to the Quality Assurance sources. It returns the list of the Quality Assurance sources.

```json
[

  {
    id: "openaire",
    type: "qualityassurancesource",
    totalEvents: "33"
  },
  {
    id: "another-source",
    type: "qualityassurancesource",
    lastEvent: "2020/10/09 10:11 UTC",
    totalEvents: "21"
  },
  ...
]
```
Attributes:
* lastEvent: the date of the last update from the specific Quality Assurance source
* totalEvents: the total number of quality assurance events provided by the Quality Assurance source
* id: is the identifier to use in GET Single Source. It can be composed of a source alias followed by colon : and the target uuid when the qa source data are focused on a specific item

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access

## GET Single Source
**GET /api/integration/qualityassurancesources/<:qualityassurancesource-id>**
​
Provide detailed information about a specific Quality Assurance source. The JSON response document is as follow
​
```json
{
  id: "openaire",
  type: "qualityassurancesource",
  totalEvents: "33"
}
 ```
​
Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access
* 404 Not found - if the source doesn't exist

## Search methods
### Get qualityassurancesources by a given target
**/api/integration/qualityassurancesources/search/byTarget**

It returns the list of qa sources that have events related to the specific target.

```json
...
_embedded: {
  qualityassurancesources:
  [

    {
      id: "openaire:<target-uuid>",
      type: "qualityassurancesource",
      totalEvents: "3"
    },
    {
      id: "coar-notify:<target-uuid>",
      type: "qualityassurancesource",
      totalEvents: "2"
    },
    ...
  ]
}
```

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* source: mandatory, the name associated with a specific source

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the topic parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access


