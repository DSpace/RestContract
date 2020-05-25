# Configuration properties

## Main Endpoint
**/api/config/properties**   

As we don't have yet an use case to iterate over all the configuration properties the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single property
**/api/config/properties/<:property>**

This endpoint provides functionality to retrieve certain (configurable) configuration properties. 
The DSpace server will have a whitelist of properties that can be retrieved by this endpoint.


```json
{
  "name": "google.analytics.key",
  "values": [ 
      "UA-XXXXXX-X"
   ]
}
```          

* 200 OK - if the operation succeed
* 404 Not found - if the property doesn't exist or isn't configured to be retrieved