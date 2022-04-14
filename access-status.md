# Access Status
[Back to the list of all defined endpoints](endpoints.md)

This endpoint expose the mechanism for retrieving and calculating the access status of a DSpace item.

## Find
**/api/accessStatus/find{?uuid}**

The access status can be checked by calling this endpoint with the the corresponding item UUID.

```
curl -v "http://{dspace-server.url}/api/accessStatus/find?uuid=2245f2c5-1bed-414b-a313-3fd2d2ec89d6"
```

This will return the access status, E.G.:

_Response if the UUID parameter is valid_
```json
{
  "status": "metadata.only",
  "type": "accessStatus"
}
```

_Response if the UUID parameter is invalid_
```json
{
  "status": null,
  "type": "accessStatus"
}
```

_Response if the UUID parameter is missing_
```json
{
  "timestamp": "2022-04-14T19:00:58.407+00:00",
  "status": 404,
  "error": "Not Found",
  "trace": "org.dspace.app.rest.exception.RepositoryNotFoundException: ...",
  "path": ".../api/accessStatus/find"
}
```

Fields
- Status: String value if the UUID is valid and a null value if it's not
- Type: Type of the endpoint, "accessStatus" in this case

Default access status values
- metadata.only
- open.access
- embargo
- restricted

Return code
- 200 Ok if the parameter is a valid item UUID (the item can be retrieved with this identifier)
- 404 Not Found if the item cannot be retrieved or if the UUID parameter is missing 
