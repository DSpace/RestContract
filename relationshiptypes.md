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

## Search methods

### Relationship types containing an entity type
**/api/core/relationshiptypes/search/byEntityTypeId?id=<:entity-type-id>**

Parameters:
* The `id` should be the entity type id from the [entity types endpoint](entitytypes.md). It is mandatory. It can occur on either the left or right hand side

A sample search would be /server/api/core/relationshiptypes/search/byEntityTypeId?id=1

It would respond with
```json
{
  "_embedded": {
    "relationshiptypes": [
      {
        "id": 10,
        "leftwardType": "isAuthorOfPublication",
        "rightwardType": "isPublicationOfAuthor",
        "copyToLeft": false,
        "copyToRight": false,
        "leftMinCardinality": 0,
        "leftMaxCardinality": null,
        "rightMinCardinality": 0,
        "rightMaxCardinality": null,
        "type": "relationshiptype"
      },
      {
        "id": 1,
        "leftwardType": "isAuthorOfPublication",
        "rightwardType": "isPublicationOfAuthor",
        "copyToLeft": false,
        "copyToRight": false,
        "leftMinCardinality": 0,
        "leftMaxCardinality": null,
        "rightMinCardinality": 0,
        "rightMaxCardinality": null,
        "type": "relationshiptype"
      }
    ]
  }
}
```

## Property-based projections

[Property-based projections](projections.md#property-based-projections) can add new JSON properties to the response. When requesting the projection, any `relationshiptype` in the response will add these properties.  
The REST request doesn't have to be on the `relationshiptype` endpoint directly, it can be on another endpoint which simply embeds relationship types.

### Verify whether a given entity type is the left or right
**?projection=CheckSideEntityInRelationshipType&checkSideEntityInRelationshipType=<:entitytype>**

This is a projection for relationship types, to indicate on which side of the relationship type the given entity type resides

When using the `projection=CheckSideEntityInRelationshipType`, it is possible to check on which side the given entity type resides.
The parameter `checkSideEntityInRelationshipType` determines which entity type should be checked.
The parameter `checkSideEntityInRelationshipType` is not repeatable.

Sample value:
* `checkSideEntityInRelationshipType=Publication`: check whether the current relationship type contains the type `Publication` on the left or right side

Response:
* The response will contain 2 extra JSON properties `relatedTypeRight` and `relatedTypeLeft` in the relationship type object
* Both new properties are booleans, defaulting to false
* If the given entity type occurs on the left side, `relatedTypeLeft` will be set to true
* If the given entity type occurs on the right side, `relatedTypeRight` will be set to true
* If the given entity type doesn't occur, both will be false
