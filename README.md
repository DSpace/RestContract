# DSpace 7 REST Contract

This repository documents new DSpace REST API Contract beginning with version 7.0.
* The code that implements this contract is on the  [`main` branch](https://github.com/DSpace/DSpace/tree/main/dspace-server-webapp) of the DSpace backend.
* One example client which utilizes this contract is the [DSpace User Interface](https://github.com/DSpace/dspace-angular/), built in [Angular.io](https://angular.io/).

## Table of Contents
* [REST Endpoints](#rest-endpoints)
* [Use of HTTP Verbs and Response Codes](#use-of-http-verbs-and-response-codes)
    * [On Collection of Resource Endpoints](#on-collection-of-resources-endpoints)
    * [On Single Resource Endpoints](#on-single-resource-endpoints)
    * [On Sub-Path of a Single Resource Endpoint (Associations)](#on-sub-path-of-a-single-resource-endpoint-associations)
    * [Error Codes](#error-codes)
* [Pagination](#pagination)
    * [Pagination Request Parameters](#pagination-request-parameters)
    * [Pagination Response](#pagination-response)
    * [Out of Bound Pages](#out-of-bound-pages)
    * [Pagination Error Codes](#pagination-error-codes)
* [REST Design Principles](#rest-design-principles)
    * [On the Naming of Endpoints](#on-the-naming-of-endpoints)
    * [HATEOAS & HAL](#hateoas--hal)
    * [Statelessness](#statelessness)
        * [JSON Web Tokens](#json-web-tokens)
    * [ALPS](#alps---application-level-profile-semantics)
    * [Spring Technology Alignment](#spring-technology-alignment)
* [Content Negotiation](#content-negotiation)
    * [Language Support](#language-support)
* [Proxies](#proxies)
* [ETags & Conditional Headers](#etags--conditional-headers)
* [API Versioning](#api-versioning)
* [Community Resources](#community-resources)

## REST Endpoints
At the ROOT of the API a HAL document lists all the primary endpoints allowing full discovery of the API.
* [Endpoints](endpoints.md) - Documentation for all available endpoints
* [Submission / Deposit](submission.md) - How to deposit (or submit) a new Item into DSpace.
* [Search Options and Relationships (on Endpoints)](search-rels.md) - How to use `/search` sub-paths on many endpoints
* [Projections (on Endpoints)](projections.md) - How to use projections to return a subset or custom view of content
* [CSRF Protection (on Endpoints)](csrf-tokens.md) - When using modifying verbs (POST, PUT, PATCH, DELETE), you *must* pass a CSRF token in the request.

## Use of HTTP Verbs and Response Codes

_Please note that within this section, all terms used are meant to reference RESTful terminology and/or terminology borrowed from [Spring Data REST](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#repository-resources)._ 
 - `resource` - anything that can be identified via a URI. It's a representation of an underlying object. e.g. `/items/123-456-789` is a resource that represents a single DSpace Item object. For additional examples see https://restful-api-design.readthedocs.io/en/latest/resources.html
 - `object` - we use this term to mean the actual object *behind* the resource. In other words, a resource is a JSON representation of some object (often a DSpaceObject) stored in the database. This is also sometimes called an "entity" in REST terminology.
 - `collection` - a group of resources e.g. `/items` is a collection of all DSpace Item resources. This is also sometimes called a "collection resource" (e.g. in Spring Data REST)
     - Be aware that the term "collection" in this document has nothing to do with a DSpace Collection object. In REST terminology, a "collection" endpoint is simply an endpoint that returns multiple resources (objects), often in a paginated list.
     - NOTE: Spring Data REST likes to use the phrase "item resources" for individual resources within a "collection resource". We've simplified this terminology and just call them "resources" and "collections", respectively.
 - `association` - a link or relationship between two resources. This is also sometimes called an "association resource" (e.g. in Spring Data REST).
 
### On Collection of Resources Endpoints

This type of endpoint interacts with a group (or collection) of resources (objects). For example: `/api/core/items` references *all* Items in the system.

- `POST` -
Creates a new resource (object) and adds it to the group. Any required data (i.e. attributes) for the new object _must be included_ in the request body. An empty request body is not allowed, unless you are creating an empty object with no attributes.
    - Related or additional information (such as required associations to other objects) may be passed as querystring parameters in the request, _if it is required to create the object._
        ```
        # For example, creating a Collection *requires* linking it to a parent Community
        # In this scenario, we must have a querystring param to specify the parent Community UUID
        curl -i -X POST "http://localhost:8080/rest/api/core/collections?parent=<:communityUUID>" 
            -H "Content-Type:application/json" 
            -d "[{ ... attributes of new Collection ... }]"
        ```

- `GET` -
Returns the first page of the resources in the group (or collection). See [Pagination](#pagination) section. 

- `PATCH` -
  Allows for batch updates (e.g. batch deletion via Patch 'remove' operation) to several resources in the collection at once.
  The request body must adhere to the the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902).
  See [General rules for the Patch operation](patch.md) for more details.

### On Single Resource Endpoints

This type of endpoint interacts with a single, existing resource (object). For example: `/api/core/items/<:uuid>` references a single Item resource.

- `GET` -
Returns a single resource.

- `HEAD` -
Returns whether the target resource is available.

- `PUT` -
Replaces the state of the target resource with the supplied request body. This updates the object (via full replacement). The updated information _must be included_ in the request body. An empty request body is not allowed, unless you are updating the object to be an empty object (with no attributes). Querystring parameters are not allowed, as `PUT` requests should always be performed on an existing object.

- `PATCH` -
Similar to PUT but partially updating the resources state. We adhere to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902).
See [General rules for the Patch operation](patch.md) and [Modifying metadata via Patch](metadata-patch.md) for more details.

- `DELETE` -
Deletes the target resource (object).

### On Sub-Path of a Single Resource Endpoint (Associations)

This type of endpoint interacts with (one or more) resources that are *associated with* a single resource. For that reason, it is sometimes called an "association" endpoint. For example: `/api/core/items/<:uuid>/mappedCollections` references all Collections that are mapped to a single Item resource.

- `GET` -
Returns the state of the association (for the current resource)

- `PUT` -
Binds (or links) the resource(s) pointed to by the given URI(s) to the current resource. This replaces any existing links with the URIs passed in the request. Resource URIs to link must be sent in the body of the request using `Content-Type:text/uri-list`. [See example from Spring Data REST Relationships](https://www.baeldung.com/spring-data-rest-relationships). Return `400 Bad Request` if multiple URIs were given for a *-to-one-association
   ```
   # Example curl command to replace Item-to-Collection mappings with the two listed
   # Notice the two Collection URIs are separated by a newline (\n)
   curl -i -X PUT "http://localhost:8080/rest/api/core/items/<:uuid>/mappedCollections" 
        -H "Content-Type:text/uri-list" 
        -d "http://localhost:8080/rest/api/core/collections/1c11f3f1-ba1f-4f36-908a-3f1ea9a557eb \n http://localhost:8080/rest/api/core/collections/5ad50035-ca22-4a4d-84ca-d5132f34f588"
   ```

- `POST` -
Only supported for collection associations (i.e. associations allowing for multiple resources). Adds/Links a new resource (via its URI) to the association. Resource URIs to link must sent in the body of the request using `Content-Type:text/uri-list`. [See example from Spring Data REST Relationships](https://www.baeldung.com/spring-data-rest-relationships)
   ```
   # Example curl command to add a *new* Collection mapping for an Item
   curl -i -X POST "http://localhost:8080/rest/api/core/items/<:uuid>/mappedCollections" 
        -H "Content-Type:text/uri-list" 
        -d "http://localhost:8080/rest/api/core/collections/5ad50035-ca22-4a4d-84ca-d5132f34f588"
   ```

- `DELETE` -
Unbinds (unlinks) the association. Return `405 Method Not Allowed` if the association is required (and cannot be removed).  DELETE requests should _not_ contain a request body as some clients do not support sending a DELETE request with a body. Therefore, if an individual association needs to be removed, the DELETE request should reference that individual association in the URI of the request. For example:
   ```
   # Example curl command to delete a *single* Collection mapping for an Item
   curl -i -X DELETE "http://localhost:8080/rest/api/core/items/<:uuid>/mappedCollections/<:collection_uuid>"
   
   # Example curl command to delete *ALL* Collection mappings for an item (unsupported at this time)
   curl -i -X DELETE "http://localhost:8080/rest/api/core/items/<:uuid>/mappedCollections
   ```

### Error Codes
* `400 Bad Request` - if multiple URIs were given for a to-one-association
* `401 Unauthorized` (Unauthenticated) - if the request requires a logged-in user
* `403 Forbidden` - if the requester doesn't have enough privilege to execute the request, or the request requires [CSRF protection](csrf-tokens.md) and the CSRF token was missing or invalid.
* `404 Not Found` - if the requested entity or collection doesn't exist
* `405 Method Not Allowed` - if the method is not implemented, or a DELETE method is called on a non-optional association
* `422 Unprocessable Entity` - if the request is well-formed, but is invalid based on the given data. For example, if you attempt to create a resource under a non-existent parent resource, or attempt to update a read-only (non-editable) field.

## Pagination
Each endpoint that exposes a collection of resources, including sub-paths for embedded or linked collections (aka list of items of a collection, etc.), MUST implement the pagination with the following common behavior

### Pagination Request Parameters
- `page` - zero-based integer value that specify the requested page in the result set (default 0). Negative values must be rejected with a `400` Error code.
- `size` - the dimension of the result set window to show. It must be a positive value. Negative or zero values must be rejected with a `400` Error code. The default value as well as maximum values are configurable by the system administrator. Different Maximum values apply for anonymous users, logged-in users and administrators. Size over the maximum value are automatically reset to the maximum allowed value, no error is thrown.
- `sort` - the criteria to use. Ordering is specified appending a comma and the keyword asc or desc to the criteria name (i.e. title,asc). Unknown sort criteria and/or ordering keyword produce an error response with Http Code `400`

### Pagination Response
The HAL document always includes a page object with the following attributes
- `size` - the dimension of the result set window returned (can be different from the requested value due to imposed limit, see the request parameters section)
- `totalElements` - the total size of the result set
- `totalPages` - the number of available page of result using the current window size
- `number` - the index (zero-based) of the returned page

An example
```
"page": {
    "size": 5,
    "totalElements": 14,
    "totalPages": 3,
    "number": 0
}
```
When applicable, the following links may also appear:
- `self` - a parameterized link to the requested collection page
- `next` - the link to the next page of resources in the collection, if any, keeping the same option for size and sorting
- `previous` - the link to the previous page of resources in the collection, if any, keeping the same option for size and sorting
- `first` - the link to the first page of resources in the collection, keeping the same option for size and sorting
- `last` - the link to the last page of resources in the collection, keeping the same option for size and sorting

An example
```
"_links": {
    "first": {
      	"href": "http://localhost:8080/server/api/core/bitstreams?page=0&size=5"
    },
    "self": {
      	"href": "http://localhost:8080/server/api/core/bitstreams"
    },
    "next": {
      	"href": "http://localhost:8080/server/api/core/bitstreams?page=1&size=5"
    },
    "last": {
        "href": "http://localhost:8080/server/api/core/bitstreams?page=2&size=5"
    }
}
```

### Out of Bound Pages
If the request parameters lead to a page outside the result set, then an empty page should be returned with the links needed to go to the first and last page of the result set (if the results set is not empty), and the total number of resources in the collection.

### Pagination Error Codes
`400 Bad Request` - If an unknown sort criteria is requested, or a not valid ordering keyword is specified

## REST Design Principles
In the creation of the REST API, we've tried to follow a few specific design principles listed below

### On the Naming of Endpoints

We strive to align with these rules for all REST API endpoint names:
* Endpoint names MUST use plural nouns instead of verbs.
* When compounds are used in an endpoint name, the words MUST be concatenated, all lowercase, without punctuation. For example: `metadatafields`, NOT `metadata-fields` or `MetadataFields` or `metadataFields`.
* Endpoint names SHOULD be descriptive, but reasonably short (<25 characters). Abbreviations are _not recommended_, unless necessary for brevity. When abbreviations are used, the REST Contract MUST spell out the meaning of the abbreviation.
* Endpoints SHOULD be grouped with other related endpoints by category or name. 
    * The "category" is the first part of the endpoint's path and is always a singular noun. For example, all configuration-related endpoints use the category "config", which means they appear under `/api/config/*`.  Similarly, all submission-related endpoints use the category "submission", which means they appear under `/api/submission/*`.
    * Alternatively, the name of the endpoint may include a prefix to relate it to other endpoints.  For example, all workflow related endpoints start with the prefix "workflow", regardless of their category. For example: `/api/config/workflowdefinitions`, `/api/config/workflowsteps`, `/api/workflow/workflowitems`.

### HATEOAS & HAL
The REST API supports the [HATEOAS paradigm](https://restfulapi.net/hateoas/) and adopt the [HAL format](http://stateless.co/hal_specification.html) to express links and embedded resources. Links are always **absolute** to allow for easier implementation of "follow link" methods on the REST client side.

Because the API responds using the HAL Format, we distribute it with an [embedded HAL Browser provided by Spring](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#_the_hal_browser).

### Statelessness
The REST API is [stateless](https://restfulapi.net/statelessness/), meaning the client is responsible for keeping state information and sending it to the server when it is necessary. Because the server stores no state (or session) information internally, it will return a "token" (which contains any state information) to the client. The client must return that token in *each subsequent request*, if the client wants that state information preserved.

#### JSON Web Tokens
[JSON Web Tokens (JWT)](https://jwt.io/) are used to store state (and authentication) information between requests. This is the format of token the REST API returns to the client. The client should return the JWT to the server in subsequent requests.

### ALPS - Application Level Profile Semantics
**While not yet implemented**, we expect future support for the ALPS metadata (<http://alps.io/>), so a profile link MUST exist from the root of API.
A profile link as defined in [RFC 6906](<https://tools.ietf.org/html/rfc6906>), is a place to include application level details. See the ALPS draft spec (http://tools.ietf.org/html/draft-amundsen-richardson-foster-alps-00)

### Spring Technology Alignment
While it's an implementation detail, the new REST API uses many Spring REST (Java) libraries, including:

* [Spring Boot](https://spring.io/projects/spring-boot) - Provides base Spring webapp functionality
* [Spring MVC](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html#mvc) - Provides Spring web framework
* [Spring REST](https://spring.io/understanding/REST) - Provides base REST tools/libraries for building REST APIs on Spring MVC
* [Spring HATEOAS](https://spring.io/projects/spring-hateoas) - Provides tools to create REST APIs following HATEOAS principles, and returning HAL Format responses.
* [Spring Data REST](https://spring.io/projects/spring-data-rest) (_alignment_) - We do **NOT** directly use Spring Data REST at this time (because of incompatibilities with our data layer). However, we have chosen to _align our implementation with Spring Data REST_ where possible.
* [Spring Data REST Hal Browser](https://docs.spring.io/spring-data/rest/docs/current/reference/html/#_the_hal_browser) - Provides the out-of-the-box HAL Browser you see when accessing our REST API
* [Spring REST Docs](https://spring.io/projects/spring-restdocs) - Provides tools to help document REST APIs in Spring MVC

Each of these libraries were chosen based on design principles above, and based on the fact that much of our underlying Java APIs use or align with Spring technologies.

## Content Negotiation
The DSpace REST API only supports the `application/json` response format at this time. So, there is no real content negotiation according to the broad meaning of the term.

Nevertheless, some concepts of content negotiation still apply.
* `POST` requests may accept a `content-type` other than `application/json` in order to drive the creation of the resource. For example, `text/uri-list` and `multipart/form-data` may be supported.
* `PUT` requests on the association endpoint accept `text/uri-list` according to Spring Data best practices.

### Language Support
While most i18n (internationalization) settings exist in the UI layer, there are some backend features which also require i18n, such as Submission forms, Controlled Vocabularies, and Authorities.

For such features the `Accept-Language` header may be used by the REST client to force the use of a specific locale, if supported by the DSpace instance and endpoint in the response. (Please note that not all the DSpace instances support multiple locales. This can be discovered by querying the configuration endpoint for the `webui.supported.locales` key.)

If the support for multiple locales is enabled in the DSpace instance, a request with an `Accept-Language` header is expected to be processed as follows:
* If none of the requested locales is supported, the server will ignore the header. This provides more user-friendly behavior when browsing the REST API via a browser (see https://tools.ietf.org/html/rfc7231#section-5.3.5)
* If at least one acceptable locale is requested, the server will use the one with the higher priority in the `Accept-Language` header to process the request
* If the requested endpoint is configured to respond differently based on the selected locale, then the `Content-Language` response header will detail which locale was used to generate the response.
* If the requested endpoint responds the same no matter which locale is requested, then the `Content-Language` response will return the list of all supported locales.
    * This also implies that the ROOT endpoint will return the list of supported locales via the `Content-Language` response header

The REST client is expected to use the `Content-Language` Response Header as part of their caching strategy.

Please note that a REST client MUST always send the `Accept-Language` header for all the subsequent requests _after_ a user has chosen a preferred language/locale.

If no explicit locale is requested by the client, or the requested locale is not supported, then DSpace will check if the current user is authenticated and has a preferred language stored (if any).  If so, DSpace will respond in the preferred locale. If not (or if the user is not authenticated), DSpace will respond with the configured default locale (configuration key `default.locale`) as a fallback.

## Proxies
The DSpace REST API supports the `X-Forwarded-For` header by default for all requests coming from localhost (127.0.0.1). This allows any clients running on the same server to immediately proxy requests for other IP addresses.

If your client (or User Interface) is on a different server, you may add additional "trusted" IP addresses (or ranges) which are allowed to use the `X-Forwarded-For` header. To do so, modify the `proxies.trusted.ipranges` setting in your `local.cfg`. This configuration accepts a comma-separated list of IPs and/or you may specify a range by listing the first three blocks of the IP range (e.g. 123.45.67)

Keep in mind, the `X-Forwarded-For` header is ONLY read if `useProxies=true` (default value) is also set in your DSpace configuration (either `dspace.cfg` or `local.cfg`).

## ETags & conditional headers
The ETag header (<http://tools.ietf.org/html/rfc7232#section-2.3>) provides a way to tag resources. This can prevent clients from overriding each other while also making it possible to reduce unnecessary calls. It is expected that all the returned document have an ETag

The ETag value can be used with in GET request with the *If-None-Match* conditional header. If the header MATCHES the ETag, the API will conclude nothing has changed, and instead of sending a copy of the resource, an HTTP 304 Not Modified status code is returned.

The ETag value can be also used with DELETE, POST, PUT and PATCH with the *If-Match* conditional header to avoid performing an action on changed resources (concurrency issues, optimistic lock approach).

Finally, when possible the If-Modified-Since header in GET request should be respected. If the resource has been not modified since the value of the Header the API should return an
HTTP 304 Not Modified status code. Resources that support the *If-Modified-Since* header *MUST* return the Last-Modified Header in the GET response. That header *MUST BE NOT* returned by resources not able to manage the If-Modified-Since header.

## API Versioning

At this time, DSpace REST API is versioned alongside DSpace software (e.g. DSpace v7.x comes with REST API v7).
Please check the Release Notes for information around any deprecations / breaking changes to the API between major versions of DSpace.

## Community Resources
* [REST Code Branch](https://github.com/DSpace/DSpace/tree/main/dspace-server-webapp)
* [REST Coding Tips](https://wiki.lyrasis.org/display/DSPACE/DSpace+7+REST:+Coding+DSpace+Objects)
* [DSpace Community Slack](https://wiki.lyrasis.org/display/DSPACE/Slack) (See `#rest-api` channel)
* [DSpace 7 Working Group](https://wiki.lyrasis.org/display/DSPACE/DSpace+7+Working+Group) - Includes weekly meeting agenda/notes and historical documentation behind the REST API
