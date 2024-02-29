## Main Endpoint
Provide access to the Quality Assurance topics. A topic represents a specific type of event (such as a missing abstract can be).

**GET /api/integration/qualityassurancetopics**
This method is not implemented as we haven't a use case that require to iterate over all the qa topics regardless of their source. Please use the search/bySource method instead.


```json
  {
    key: "ENRICH!MORE!PID",
    lastEvent: "2020/10/09 10:11 UTC",
    totalEvents: "33"
  },
  {
    key: "ENRICH!MISSING!ABSTRACT",
    lastEvent: "2020/10/09 10:11 UTC",
    totalEvents: "21"
  },
  ...
]
```
Attributes:
* key: the name of the topic to display on the frontend user interface
* lastEvent: the date of the last update from Quality Assurance Broker
* totalEvents: the total number of quality assurance events provided by Quality Assurance Broker for this topic


Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access

## GET Single Topic
**GET /api/integration/qualityassurancetopics/<:qualityassurancetopic-id>**
​
Provide detailed information about a specific Quality Assurance Broker topic. The JSON response document is as follow
​
```json

{
  id: "openaire:ENRICH!MORE!PID",
  type: "qualityassurancetopic",
  name: "ENRICH/MORE/PID",
  lastEvent: "2020/10/09 10:11 UTC",
  totalEvents: 33
}
```

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access
* 404 Not found - if the topic doesn't exist

## Search methods
### Get qualityassurancetopics by a given source
**/api/integration/qualityassurancetopics/search/bySource**

It returns the list of qa topics from a specific source. Provide paginated list of the qa topics available.

```json
...
_embedded: {
  qualityassurancetopics:
  [

    {
      id: "openaire:ENRICH!MORE!PID",
      type: "qualityassurancetopic",
      name: "ENRICH/MORE/PID",
      lastEvent: "2020/10/09 10:11 UTC",
      totalSuggestions: "33"
    },
    {
      id: "openaire:ENRICH!MISSING!ABSTRACT",
      type: "qualityassurancetopic",
      name: "ENRICH/MISSING/ABSTRACT",
      lastEvent: "2020/10/09 10:11 UTC",
      totalSuggestions: "21"
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


### Get qualityassurancetopics by a given target
**GET /api/integration/qualityassurancetopics/search/byTarget?target=:item-uuid&source=:source-id**

It returns the list of qa topics (from a specific source) to a specific targeted item.

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the target or the source parameters are missing or invalid (the target is not an uuid)
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions, only system administrators can access
