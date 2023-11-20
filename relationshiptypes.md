# Relationship Type Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains the various types of relationships.
A sample is a relation between a publication and a person with type isAuthorOfPublication
A HAL link to the item types is embedded as well

## Main Endpoint
**/api/core/relationshiptypes**

A sample can be found at https://demo.dspace.org/#https://demo.dspace.org/server/api/core/relationshiptypes

## Single Relationship Type
**/api/core/relationshiptypes/<:id>**

A sample can be found at https://demo.dspace.org/#https://demo.dspace.org/server/api/core/relationshiptypes/1

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
      "href": "https://demo.dspace.org/server/api/core/entitytypes/1"
    },
    "rightType": {
      "href": "https://demo.dspace.org/server/api/core/entitytypes/2"
    },
    "self": {
      "href": "https://demo.dspace.org/server/api/core/relationshiptypes/1"
    }
  },
  "_embedded": {
    "leftType": {
      "id": 1,
      "label": "Publication",
      "type": "entitytype",
      "_links": {
        "self": {
          "href": "https://demo.dspace.org/server/api/core/entitytypes/1"
        },
        "relationshiptypes": {
          "href": "https://demo.dspace.org/server/api/core/entitytypes/1/relationshiptypes"
        }
      }
    },
    "rightType": {
      "id": 2,
      "label": "Person",
      "type": "entitytype",
      "_links": {
        "self": {
          "href": "https://demo.dspace.org/server/api/core/entitytypes/2"
        },
        "relationshiptypes": {
          "href": "https://demo.dspace.org/server/api/core/entitytypes/2/relationshiptypes"
        }
      }
    }
  }
}
```

The 2 [item types](itemtypes.md) are embedded

## Search methods

### Relationship types containing an entity type
**/api/core/relationshiptypes/search/byEntityType?type=<:entity-type-label>**

Parameters:
* The `type` should be the entity type label from the [entity types endpoint](entitytypes.md). It is mandatory. It can occur on either the left or right hand side

A sample search would be `/server/api/core/relationshiptypes/search/byEntityType?type=Publication'

Assuming that the sample `config/entities/relationship-types.xml` data model has been loaded, it would respond with

```json
{
  "_embedded": {
    "relationshiptypes": [
      { // it is between Person and Publication
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
        "leftwardType": "isProjectOfPublication",
        "rightwardType": "isPublicationOfProject",
        "copyToLeft": false,
        "copyToRight": false,
        "leftMinCardinality": 0,
        "leftMaxCardinality": null,
        "rightMinCardinality": 0,
        "rightMaxCardinality": null,
        "type": "relationshiptype"
      },
      {
        "id": 7,
        "leftwardType": "isOrgUnitOfPublication",
        "rightwardType": "isPublicationOfOrgUnit",
        "copyToLeft": false,
        "copyToRight": false,
        "leftMinCardinality": 0,
        "leftMaxCardinality": null,
        "rightMinCardinality": 0,
        "rightMaxCardinality": null,
        "type": "relationshiptype"
      },
      { // this is a different relationshipttype than the one with id 10
        // as it is about Publication and OrgUnit
        "id": 17,
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
        "id": 18, 
        "leftwardType": "isPublicationOfJournalIssue",
        "rightwardType": "isJournalIssueOfPublication",
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
comments inside the above json are only included for clarity but are not part of the real response.

Return codes:
* 200 OK - if the operation succeed. This include the case of no matching relationship where a 0-size page json representation is returned.
* 400 Bad Request - if the type parameter is missing or invalid
