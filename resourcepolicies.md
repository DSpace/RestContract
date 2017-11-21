# Resource Policies Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/authz/resourcepolicies**   

As we don't have yet an use case to iterate over all the existent resource policies the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/core/collections>

## Single Resource Policy
**/api/authz/resourcepolicies/<:id>**

Provide detailed information about a specific resource policy. The JSON response document is as follow
```json
{
  "id": 2844,
  "name": null,
  "description": null,
  "resourceType": "TYPE_SUBMISSION",
  "action": "DEFAULT_BITSTREAM_READ",
  "type": "resourcePolicy"
}
```

Exposed links:
* eperson: link to the eperson that have permission grant by the policy
* group: link to the group that have permission grant by the policy
* resource: link to the resource subject to the policy

## Linked entities
### EPerson
**/api/authz/resourcepolicies/<:id>/eperson**

t.b.d

### Group
**/api/authz/resourcepolicies/<:id>/group**

t.b.d

### Resource
**/api/authz/resourcepolicies/<:id>/resource**

t.b.d