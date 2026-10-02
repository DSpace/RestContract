# Search Components Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/layout/searchcomponents**   

Not implemented

## Single Collection
**/api/layout/searchcomponents/<:string>**

Provide detailed information about a specific community. The JSON response document is as follow
```json
{
  "id": "box-shortname",
  "configuration": "discovery-configuration",
  "type": "searchcomponent"
}
```

Attributes
* the *configuration* attribute will match a discovery configuration where a facet named "cluster" will be eventually defined  to group the result by arbitrary defined criteria (facet query)