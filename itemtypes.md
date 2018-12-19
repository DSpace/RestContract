# Item Type Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint is currently still named entitytypes, but may be renamed still.

It contains the various types of items (publication, person, journal, …)

## Main Endpoint
**/api/core/entitytypes**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/entitytypes

## Single Item Type
**/api/core/entitytypes/<:id>**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/entitytypes/1

```json
{
  "id": 1,
  "label": "Publication",
  "type": "entitytype",
  "_links": {
    "self": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/1"
    },
    "relationshiptypes": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/1/relationshiptypes"
    }
  }
}
```

It contains a HAL link to the Relationship Types for the current Item Type (not embedded)

## Relationship Types for the current Item Type
**/api/core/entitytypes/<:id>/relationshiptypes**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/entitytypes/1/relationshiptypes
It embeds the [relationshiptypes](relationshiptypes.md)
