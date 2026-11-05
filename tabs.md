# Tabs Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/layout/tabs**   

Not implemented

## New Tab
**POST /api/layout/tabs**
To create a new Tab perform as POST with the follow JSON:

```json
{
  "shortname": "info",
  "header": "Profile",
  "entityType": "Person",
  "priority": 1,
  "security": 0,
  "type": "tab",
  "uniqueType": "layout.tab",
  "leading": "true",
  "rows": [
    {
      "cells": [
        {
          "style": "col-md-6",
          "boxes": [
            {
              "shortname": "primary",
              "header": "Primary Information",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "security": 0,
              "boxType": "METADATA",
              "metadataSecurityFields": [],
              "configuration": {
                "type": "boxmetadataconfiguration",
                "rows": [
                  {
                    "fields": [
                      {
                        "metadata": "dc.title",
                        "label": "Name",
                        "fieldType": "metadata"
                      },
                      { 
                        "metadata": "person.email",
                        "label": "Email",
                        "fieldType": "metadata"
                      }
                    ]
                  }
                ]
              }
            },
            {
              "shortname": "other",
              "header": "Other Informations",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "container": false,
              "security": 0,
              "boxType": "METADATA",
              "metadataSecurityFields": [
                "dspace.policy.eperson"
              ],
              "configuration": {
                "type": "boxmetadataconfiguration",
                "rows": [
                  {
                    "fields": [
                      {
                        "metadata": "person.birthDate",
                        "label": "Birth date",
                        "fieldType": "metadata"
                      }
                    ]
                  }
                ]
              }
            }
          ]
        },
        {
          "style": "col-md-6",
          "boxes": [
            {
              "shortname": "researchoutputs",
              "header": "Research outputs",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "container": true,
              "security": 0,
              "boxType": "RELATION",
              "metadataSecurityFields": []
            }
          ]
        }
      ]
    },
    {
      "style": "bg-light",
      "cells": [
        {
          "style": "col-md-12",
          "boxes": [
            {
              "shortname": "metrics",
              "header": "Metrics",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": null,
              "security": 0,
              "boxType": "METRICS",
              "metadataSecurityFields": [],
              "configuration": {
                "type": "boxmetricsconfiguration",
                "maxColumns": 2,
                "metrics": ["views", "downloads"]
              }
            }
          ]
        }
      ]
    }
  ]
}
```

This endpoint is reserved to system administrators, its returns the created tab.

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 422 UNPROCESSABLE ENTITY - if the json body is unprocessable for DynamicLayoutTabRest entity

## Delete tab
**DELETE /api/layout/tabs/<:id>**

Delete a Tab. This endpoint is reserved to system administrators.

Return codes:
* 204 No content - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
* 404 Not found - if the tab doesn't exist (or was already deleted)

## Single Tab
**GET /api/layout/tabs/<:id>**

Provide detailed information about a specific tab. The JSON response document is as follow

```json
{
  "id": 1,
  "shortname": "info",
  "header": "Profile",
  "entityType": "Person",
  "priority": 1,
  "security": 0,
  "type": "tab",
  "uniqueType": "layout.tab",
  "leading": "true",
  "rows": [
    {
      "cells": [
        {
          "style": "col-md-6",
          "boxes": [
            {
              "shortname": "primary",
              "header": "Primary Information",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "container": true,
              "maxColumns": 2,
              "security": 0,
              "boxType": "METADATA",
              "metadataSecurityFields": [],
              "configuration": {
                "id": 1,
                "rows": [
                  {
                    "fields": [
                      {
                        "metadata": "dc.title",
                        "label": "Name",
                        "fieldType": "metadata"
                      },
                      { 
                        "metadata": "person.email",
                        "label": "Email",
                        "fieldType": "metadata",
                        "valuesInline": "true"
                      }
                    ]
                  }
                ]
              }
            },
            {
              "shortname": "other",
              "header": "Other Informations",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "maxColumns": 2,
              "security": 0,
              "boxType": "METADATA",
              "metadataSecurityFields": [
                "dspace.policy.eperson"
              ],
              "configuration": {
                "id": 2,
                "rows": [
                  {
                    "fields": [
                      {
                        "metadata": "person.birthDate",
                        "label": "Birth date",
                        "fieldType": "metadata",
                        "labelAsHeading": "true"
                      }
                    ]
                  }
                ]
              }
            }
          ]
        },
        {
          "style": "col-md-6",
          "boxes": [
            {
              "shortname": "researchoutputs",
              "header": "Research outputs",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": "col-md-6",
              "maxColumns": 2,
              "security": 0,
              "boxType": "RELATION",
              "metadataSecurityFields": [],
              "configuration": {
                "id": 3,
                "discovery-configuration": "RELATION.Person.researchoutputs"
              }
            }
          ]
        }
      ]
    },
    {
      "style": "bg-light",
      "cells": [
        {
          "style": "col-md-12",
          "boxes": [
            {
              "shortname": "metrics",
              "header": "Metrics",
              "entityType": "Person",
              "collapsed": false,
              "minor": false,
              "style": null,
              "maxColumns": 2,
              "security": 0,
              "boxType": "METRICS",
              "metadataSecurityFields": [],
              "configuration": {
                "id": 4,
                "numColumns": 2,
                "metrics": ["views", "downloads"]
              }
            }
          ]
        }
      ]
    }
  ]
}
```

Attributes
* the *header* attribute is the label or the i18n key to use to present the section to the user
* the *leading* attribute is a boolean that indicates if the given tab should always be visible on the top of the page
* the *security* attribute is a constant where 0 mean public, 1 mean administrators, 2 mean owner only, 3 owner & administrators, 4 custom metadata
* the *rows* attribute is an array with the ordered list of rows that compose the tab. Each row has the following attributes
  * the *style* attribute is the css style related to the row
  * the *cells* attribute is an array with the ordered list of cells that compose the row. Each cell has the following attributes
    * the *style* attribute is the css style related to the cell
    * the *boxes* attribute is an array with the ordered list of boxes that compose the cell. Each box has the following attributes
      * the *header* attribute is the label or the i18n key to use to present the section to the user 
      * the *security* attribute is a constant where 0 mean public, 1 mean administrators, 2 mean owner only, 3 owner & administrators, 4 custom metadata 
      * the *collapsed* attribute indicates whether the box should be shown initially collapsed or not 
      * the *container* attribute indicates if the box container should be shown or not
      * the *minor* attribute is used to flag box that should be ignored in the determination of the tab visualization 
      * the *style* attribute is the css style related to the box
      * the *boxType* attribute is used to choice the appropriate component. It could be metadata, search, bibliometrics 
      * the *maxColumn* attribute is used to indicates how many column should be used to visualize content (currently only metrics boxes supports this parameter). 
      * the *configuration* attribute contains more information specific for the box type. Configuration details depend on the specific [box type](boxes-types.md)
      * the *metadataSecurityFields* attribute is an array of the metadatafields that defined the security

Exposed links:
* securityMetadata: link to the metadatafields that defined the security

Return codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated. Please note that this also apply to resource policy related to the Anonymous group
* 403 Forbidden - if you are not logged in with sufficient permissions. Only system administrators, users with ADMIN right on the target resource, users mentioned in the policy (eperson or member of the group) can access the tab
* 404 Not found - if the tab doesn't exist (or was already deleted)

### SecurityMetadata
**GET /api/layout/tabs/<:id>/securitymetadata**

It returns all the metadatafields that defined the security.

Return codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated.
* 403 Forbidden - if you are not logged in as an administrator
* 404 Not found - if the tab doesn't exist (or was already deleted)

**GET /api/layout/tabs/<:id>/securitymetadata/<:id>**

_Unsupported._ If you want detailed information about a single metadatafield in the tab, use the `/api/core/metadatafields/<:metadatafield.id>` endpoint.

**POST /api/layout/tabs/<:id>/securitymetadata**

A POST request will result in adding the metadatafield to the list of metadata used to evaluate access permission to the tab.

The metadata (also more than one) MUST be included in the body using the `text/uri-list` content type
 
Return codes:
 * 204 No Content - if the update succeeded (including the case of no-op if the mapping was already as requested)
 * 401 Unauthorized - if you are not authenticated
 * 403 Forbidden - if you are not logged in as administrator 
 * 404 Not found - if the tab doesn't exist (or was already deleted)
 * 422 Unprocessable Entity - if the specified metadata is not found

**PUT /api/layout/tabs/<:id>/securitymetadata**

_Unsupported._ You may replace or update the security metadata using DELETE requests and/or POST requests.

**DELETE /api/layout/tabs/<:id>/securitymetadata**

_Unsupported._ At this time, we do not support removing all security metadata in a single request. Please use `DELETE /api/layout/tabs/<:id>/securitymetadata/<:metadatafield.id>` to remove metadatafield one by one.

**DELETE /api/layout/tabs/<:id>/securitymetadata/<:metadatafield.id>**

A DELETE request will result in removing the metadatafield from the current list.

Return codes:
 * 204 No Content if the delete succeeded (including the case of no-op if the metadata was not mapped) 
 * 401 Unauthorized - if you are not authenticated
 * 403 Forbidden - if you are not logged in as administrator
 * 404 Not found - if the tab doesn't exist (or was already deleted)
 * 422 Unprocessable Entity - if the specified uri cannot resolve to a metadatafield (if the uri is valid but the metadatafield is not found, 204 is expected)


## Search methods
### findByItem
**GET /api/layout/tabs/search/findByItem?uuid=<:item-uuid>**

It returns the tabs that are available for the specified item. The tabs are sorted by priority ascending. This are filtered based on the permission of the current user and available data. Empty tabs are filter out.

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the uuid param is missing or is not an uuid
* 401 Unauthorized - if you are not authenticated and the item is not accessible to anonymous users
* 403 Forbidden - if you are not logged in with sufficient permissions to READ the item


### findByEntityType
**GET /api/layout/tabs/search/findByEntityType?type=<:string>**

It returns the tabs that are available for the items of the specified type. This endpoint is reserved to system administrators

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the type param is missing
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions
