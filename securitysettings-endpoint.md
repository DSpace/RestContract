# Security Settings Endpoints
[Back to the list of all defined endpoints](endpoints.md)

DSpace has functionality to show or hide the metadata related with an item in order to manage the security related with
this item. DSpace, also offers to the user the possible levels of security, to define for each metadata related with an
entity type. These levels are configured in configuration files and returned to che client by an endpoint, based on the
entity type requested. This contract describes this endpoint.

## Security Settings Endpoint

**GET /api/core/securitysettings/<EntityType>

This endpoint will list all (REST supported) configurations defined in `dspace/config/modules/metadata-security.cfg`,
based on an entity type. The configuration settings entries are embedded with a metadataSecurityDefault,
metadataCustomSecurity and a self link, where metadataSecurityDefault is the fallback level of security, or the level of
security for an EntityType, metadataCustomSecurity are all the configuration levels of the metadata related with an
EntityType.

The JSON response document is as follows

```json
{
  "_embedded": {
    "id": "securitysetting",
    "type": "securitysetting",
    "metadataSecurityDefault": [
      2
    ],
    "metadataCustomSecurity": {
      "dc.type": [
        1, 2
      ],
      "dc.identifier.scopus": [
        1
      ],
      "oairecerif.author.affiliation": [
        0, 1
      ],
      "dc.identifier.isi": [
        1, 2
      ],
      "dc.identifier.doi": [
        1
      ]
    },
    "_links": {
      "self": {
        "href": "/api/core/securitysettings"
      }
    }
  }
}


```
Attributes
* metadataSecurityDefault: array with integers value of security configuration.
* metadataCustomSecurity: a map with key value pairs, where key is the metadata name and value is an array with integers, representing the possible security configuration levels for that metadata.
* type: string representing the type of the rest response.
* id: string representing the type of the rest response.

Return codes:
* 200 OK - if the operation succeed.
* 401 Unauthorized - if user is not authenticated.