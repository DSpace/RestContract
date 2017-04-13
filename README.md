# Rest7Contract

Repository to discuss the new REST API contract for DSpace 7. The code to implement this contract is being developed on the DSpace [`master` branch](https://github.com/DSpace/DSpace/tree/master/dspace-spring-rest)

***
:warning: **As the DSpace 7 REST API is under active development, this REST Contract is expected to change rapidly.** REST Contract contributors will be rapidly adding ideas (via Pull Requests) to this repository in order to enable the detailed discussions necessary to finalize the REST Contract. Once the REST Contract stabilizes, we will remove this warning and more actively publicize it.

**If you would like to get involved in our DSpace 7 development effort, we welcome new contributors.**
  * DSpace 7 Meetings and subteams can be found on the [DSpace 7 UI Working Group](https://wiki.duraspace.org/display/DSPACE/DSpace+7+UI+Working+Group) wiki page.
  * DSpace 7 REST API development work (corresponding to this contract) is occurring on the DSpace [`master` branch](https://github.com/DSpace/DSpace/tree/master/dspace-spring-rest)
  * DSpace 7 Angular UI work is occurring at https://github.com/DSpace/dspace-angular
***


## Community resources

* [REST Code Branch](https://github.com/DSpace/DSpace/tree/master/dspace-spring-rest)
* [Community Slack Channel](https://dspace-org.slack.com/messages/C3T5FTLNP)
* [Historical Documentation Behind this Project](https://wiki.duraspace.org/display/DSPACE/DSpace+7+UI+Working+Group)
* [DSpace 7 UI and REST Working Group Meeting Notes](https://wiki.duraspace.org/display/DSPACE/DSpace+7+UI+Working+Group)

## Detailed Documentation pages
* [Endpoints](endpoints.md)
* [Endpoint Search Options and Relationships](search-rels.md)
* [Endpoint Projections](projections.md)

## Use of the HTTP Verbs and HTTP Response CODE

### On collection of resources endpoints
- POST
Adds a new element to the collection.

- GET
Returns the first page of the resources in the collection

### On single resource endpoints
- GET
Returns a single entity.

- HEAD
Returns whether the item resource is available.

- PUT
Replaces the state of the target resource with the supplied request body.

- PATCH
Similar to PUT but partially updating the resources state.

- DELETE
Deletes the resource exposed.

### On sub-path of a single resource endpoint)
- GET
Returns the state of the association resource

- PUT
Binds the resource pointed to by the given URI(s) to the resource. Return 400 Bad Request if multiple URIs were given for a to-one-association

- POST
Only supported for collection associations. Adds a new element to the collection.

- DELETE
Unbinds the association. Return 405 Method Not Allowed if the association is non-optional

### Error codes
400 Bad Request - if multiple URIs were given for a to-one-association

401 Unauthenticated - if the request require a logged-in user

403 Unauthorized - if the requester doesn't have enough privilege to execute the request

404 Not found - if the requested entity or collection doesn't exists

405 Method Not Allowed - if the methods is not implemented or a DELETE methods is called on a non-optional association

## HATEOAS & HAL
The new REST DSpace API supports the HATEOAS paradigm and adopt the HAL format to express links and embedded resources.

## Pagination
Each endpoints that expose a collection of resources, including sub-paths for embedded or linked collections (aka list of items of a collection, etc.), MUST implement the pagination with the following common behavior

### Request parameters
- **page**: 0 based integer value that specify the requested page in the result set [default 0]. Negative values must be rejected with a 400 Error code.
- **size**: the dimension of the result set window to show. It must be a positive value. Negative or zero values must be rejected with a 400 Error code. The default value as well as maximum values are configurable by the system administrator. Different Maximum values apply for anonymous users, logged-in users and administrators. Size over the maximum value are automatically reset to the maximum allowed value, no error is thrown.
- **sort**: the criteria to use. Ordering is specified appending a comma and the keyword asc or desc to the criteria name (i.e. title,asc). Unknown sort criteria and/or ordering keyword produce an error response with Http Code 400

### Response
The HAL document always includes a page object with the following attributes
- **size**: the dimension of the result set window returned (can be different than the requested value due to imposed limit, see the request parameters section)
- **totalElements**: the total size of the result set
- **totalPages**: the number of available page of result using the current window size
- **number**: the index (0-based) of the returned page
An example

	"page": {
    	"size": 5,
    	"totalElements": 14,
    	"totalPages": 3,
    	"number": 0
  	}

and, when applicable, the following links
- **self**: a parameterized link to the requested collection page
- **next**: the link to the next page of resources in the collection, if any, keeping the same option for size and sorting
- **previous**: the link to the previous page of resources in the collection, if any, keeping the same option for size and sorting
- **first**: the link to the first page of resources in the collection, keeping the same option for size and sorting
- **last**: the link to the last page of resources in the collection, keeping the same option for size and sorting

An example

	"_links": {
    	"first": {
      	"href": "http://localhost:8080/dspace-spring-rest/api/core/bitstreams?page=0&size=5"
    	},
    	"self": {
      	"href": "http://localhost:8080/dspace-spring-rest/api/core/bitstreams"
    	},
    	"next": {
      	"href": "http://localhost:8080/dspace-spring-rest/api/core/bitstreams?page=1&size=5"
    	},
    	"last": {
	      "href": "http://localhost:8080/dspace-spring-rest/api/core/bitstreams?page=2&size=5"
	    }
  	}

### Out of bound pages
In the case that the request parameters lead to a page outside the result set an empty page should be returned with the links needed to go to the first and last page of the result set if the results set is not empty and the total number of resources in the collection

### Error Code
400 Bad Request. If an unknown sort criteria is requested or a not valid ordering keyword is specified

## ETags & conditional headers
The ETag header (<http://tools.ietf.org/html/rfc7232#section-2.3>) provides a way to tag resources. This can prevent clients from overriding each other while also making it possible to reduce unnecessary calls. It is expected that all the returned document have an ETag

The ETag value can be used with in GET request with the *If-None-Match* conditional header. If the header MATCHES the ETag, the API will conclude nothing has changed, and instead of sending a copy of the resource, an HTTP 304 Not Modified status code is returned.

The ETag value can be also used with DELETE, POST, PUT and PATCH with the *If-Match* conditional header to avoid to perform action on changed resources (concurrency issues, optimistic lock approach).

Finally, when possible the If-Modified-Since header in GET request should be respected. If the resource has been not modified since the value of the Header the API should return an
HTTP 304 Not Modified status code. Resources that support the *If-Modified-Since* header *MUST* return the Last-Modified Header in the GET response, such header *MUST BE NOT* returned by resources not able to manage the If-Modified-Since header.  

## ALPS - Application Level Profile Semantics
It is expected support for the ALPS metadata (<http://alps.io/>), so a profile link MUST exists from the root of API.
A profile link as defined in RFC 6906 (<https://tools.ietf.org/html/rfc6906>), is a place to include application level details. The ALPS draft spec (http://tools.ietf.org/html/draft-amundsen-richardson-foster-alps-00)

## Endpoints
At the root of the api a HAL document MUST list all the primary endpoints allowing full discovery of the current version of the API.

[Documentation of the defined endpoints](endpoints.md)

## API Versioning
... here we will describe our strategy to provide access overtime to a specific version of the REST API...
### Deprecated endpoints & methods
... how we want to let know the client about deprecated endpoints & methods?
### Experimentals endpoints & methods
... do we want/need to introduce endpoints keeping the right to change behavior and other aspects without face with the self-imposed guarantee about backward compatibilty and versioning?
