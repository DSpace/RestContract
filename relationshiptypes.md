# Relationship Type Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains the various types of relationships.
A sample is a relation between a publication and a person with type isAuthorOfPublication
A HAL link to the item types is embedded as well

## Main Endpoint
**/api/core/relationshiptypes**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/relationshiptypes

## Single Relationship Type
**/api/core/relationshiptypes/<:id>**

A sample can be found at https://dspace7-entities.atmire.com/rest/#https://dspace7-entities.atmire.com/rest/api/core/relationshiptypes/1

```json
{
  "id": 1,
  "leftwardType": "isAuthorOfPublication",
  "rightwardType": "isPublicationOfAuthor",
  "leftMinCardinality": 0,
  "leftMaxCardinality": null,
  "rightMinCardinality": 0,
  "rightMaxCardinality": null,
  "type": "relationshiptype",
  "_links": {
    "leftType": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/1"
    },
    "rightType": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/2"
    },
    "self": {
      "href": "https://dspace7-entities.atmire.com/rest/api/core/relationshiptypes/1"
    }
  },
  "_embedded": {
    "leftType": {
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
    },
    "rightType": {
      "id": 2,
      "label": "Person",
      "type": "entitytype",
      "_links": {
        "self": {
          "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/2"
        },
        "relationshiptypes": {
          "href": "https://dspace7-entities.atmire.com/rest/api/core/entitytypes/2/relationshiptypes"
        }
      }
    }
  }
}
```

The 2 [item types](itemtypes.md) are embedded
