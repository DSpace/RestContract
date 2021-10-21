# Entity Type Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains the various types of items (publication, person, journal, …)

## Main Endpoint
**/api/core/entitytypes**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/entitytypes

```json
{
  "_embedded": {
    "itemtypes": [
      {
        "id": 1,
        "label": "Publication",
        "type": "itemtype",
        "_links": {
          "self": {
            "href": "https://dspace7-entities.atmire.com/rest/api/core/itemtypes/1"
          },
          "relationshiptypes": {
            "href": "https://dspace7-entities.atmire.com/rest/api/core/itemtypes/1/relationshiptypes"
          }
        }
      },
      {
        "id": 2,
        "label": "Person",
        "type": "itemtype",
        "_links": {
          "self": {
            "href": "https://dspace7-entities.atmire.com/rest/api/core/itemtypes/2"
          },
          "relationshiptypes": {
            "href": "https://dspace7-entities.atmire.com/rest/api/core/itemtypes/2/relationshiptypes"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/itemtypes"
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
It embeds the [relationshiptypes](relationshiptypes.md) which are linked to the given item type (either on the left or right side)

## Get Entity type from label
**/api/core/entitytypes/label/<:entity-type-label>**

A sample request would be https://dspace7-entities.atmire.com/server/#/server/api/core/entitytypes/label/Person
The entity-type-label is mandatory

There's always at most one entity type per label.

It would respond with:
* The single entity type if there's a match
* 404 if the entity type doesn't exist

### Search methods
#### findAllByAuthorizedCollection
**/api/core/entitytypes/search/findAllByAuthorizedCollection**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
It returns a list of entity types for which there is at least one collection in which the user is authorized to submit

Return codes:
* 200 OK - if the operation succeed

#### findAllByAuthorizedExternalSource
**/api/core/entitytypes/search/findAllByAuthorizedExternalSource**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
It returns a list of entity types for which there is at least one collection in which the user is authorized to submit supported by at least one external data source provider

Return codes:
* 200 OK - if the operation succeed
