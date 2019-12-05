# General rules for the Patch operation
[Back to the index](README.md)

The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a **HTTP 200 CODE** with the new state of the patched resource as body similar to what is returned for a GET request. 

### Add
The add operation is expected to be supported by all the endpoints that implement the Patch method for all the json path not flagged as **READ-ONLY**.

Please note that according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902) a subsequent add operation on an already set attribute have the effect to replace the previous value.

This use of the add operation to replace a value could be counter intuitive but it is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)
> If the target location specifies an object member that does exist, that member's value is replaced.

### Remove
The remove operation is expected to be supported by all the endpoints that implement the Patch method for all the json path not flagged as **READ-ONLY**.
Support for remove operation is expected to be provided also to the array index path level for each attribute or list where position is meaningful. This include for instance the metadata values or the uploaded bitstreams so that an author or a bitstream can be removed individually.  
The path specified will be nullified deleting all the children objects if any.

### Replace
The replace operation is expected to be supported by all the endpoints that implement the Patch method for all the json path not flagged as **READ-ONLY**.

The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation without a previous value will return an error. See the [general errors on PATCH requests section](patch.md#error-codes)

### Move
The move operation should be supported on array attributes or linked list where the position is meaningful. This include for instance the metadata values so that authors can be reordered or the list of bitstreams.

### Test & copy
No plan to implement the test and copy operations at the current stage

## Error codes
* **405 Method Not Allowed** - if the PATCH method is not implemented
* **415 Unsupported Media Type** - if the PATCH method is called without the JSON Patch Media Type that is application/json-patch+json
* **400 Bad Request** - Malformed patch document (taken from rfc5789#section-2.2) - When the server determines that the patch document provided by the client is not properly formatted, it SHOULD return a 400 (Bad Request) response. The definition of badly formatted depends on the patch document chosen. 
* **422 Unprocessable Entity** - Unprocessable request:  Can be specified with a 422 (Unprocessable Entity) response ([RFC4918], Section 11.2) when the server understands the patch document and the syntax of the patch document appears to be valid, but the server is incapable of processing the request.  This might include attempts to modify a resource in a way that would cause the resource to become invalid; A response json object should be returned with more details about the exception t.b.d (i.e. the idx of the patch operation that fail, detail about the failure on execution such as wrong idx in an array path, unsupported patch operation, etc.)
  
