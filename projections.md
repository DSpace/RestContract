# Projections
[REST Overview Documentation](README.md)

All `GET` requests returning a HAL document support an optional *projection* argument, specifying the name
of an alternative representation of the requested resource.

Projections may add to, modify, or omit information from the default representation of a resource.

When fulfilling a request, the active projection is applied to the primary resource being requested
as well as any embedded resources.

## Projections which act on the embed section

These projections will add information to the `_embedded` section of the HAL representation.

### Standard Projections

There are two standard projections available in all DSpace 7+ instances:

#### Default Projection

The _default_ projection makes no changes to the normal representation of the resource and **excludes all
subresource embeds**, omitting altogether the `_embedded` section of the HAL representation.

This is the implicit projection for all individual resource endpoints, such as `/api/core/items/<:uuid>`,
if no projection is specified.

#### Full Projection (`?projection=full`)

The _full_ projection includes all linked subresources also embedded in the response.

Since embeds may include other embedded resources, it is important to limit the number of embed levels
alloweds. Thus, only two levels of embeds will be returned at maximum when the _full_ projection is requested.

### Custom Projections

Developers and integrators may extend the available projections for use with a DSpace instance by adding
a new uniquely-named `Projection` as a Java Spring `@Component`, and ensuring it is deployed (e.g. as a jar)
within the DSpace server webapp.

Upon successful component discovery, the projection will automatically become available for use with all
REST API endpoints using the same syntax as the standard projections (`?projection=name`).

See [the Projection javadocs](https://github.com/DSpace/DSpace/blob/master/dspace-server-webapp/src/main/java/org/dspace/app/rest/projection/Projection.java)
for more information.

### Specify Embed requests

All `GET` requests returning a HAL document support an optional *embed* argument, specifying the link path
of information to be embedded.

Embeds can only add information from the default representation of a resource.

When fulfilling a request, the active embed is applied to the primary resource being requested
as well as any embedded resources.

All linked resources which allow embeds can be retrieved using the *embed* argument, they don't have to be configured

#### Basic embeds

The most basic usage is to specify one level of embeds.
This will allow `core/items/<:uuid>?embed=bundles&embed=owningCollection` to embed the bundles and the owningCollection of the item.

The embed parameters only apply to the current resource.
When using `/api/core/communities/<:uuid>?embed=subcommunities&embed=collections`, it embeds the collections and subcommunities of the current community.
It won't embed the collections and subcommunities of subcommunities.

The supported syntax is `core/items/{uuid}?embed=bundles&embed=owningCollection` or `core/items/{uuid}?embed=bundles,owningCollection`

#### Multi-level embeds

In case there's a use case to embed sub-resources of a sub-resource, multi-level embeds can be used.

This will allow `/server/api/core/items/<:uuid>?embed=owningCollection/mappedItems/bundles/bitstreams&embed=owningCollection/logo`.
That request will embed:
* The item's owningCollection
* The mappedItems of the item's owningCollection
* The bundles of the mappedItems
* The bitstreams of the bundles of the mappedItems
* The logo of the item's owningCollection

This will **not** embed:
* The bundles of the main item
* The item's mappedCollections
* The owning collection of the mappedItems
* …

The path specified in the embed parameters will be traversed.

It is possible the path contains the same link name multiple times.  
The request `/api/core/communities/<:uuid>?embed=subcommunities/subcommunities&embed=collections` will embed:
* The community's subcommunities
* The subcommunities of the subcommunities
* The community's collections

The request `/api/core/communities/<:uuid>?embed=subcommunities/subcommunities&embed=subcommunities/collections&embed=collections` will embed:
* The community's subcommunities
* The subcommunities of the subcommunities
* The collections of the subcommunities
* The community's collections


## Property-based projections

Property-based projections can add new JSON properties to the response. When requesting the projection, any matching object in the response may add these properties.

### Standard Projections

There are no standard property-based projections.

### Custom Projections

Some custom projections exist, e.g. on the [item](items.md#verify-whether-theres-a-relationship-with-another-given-item) or [relationships](relationships.md#verify-whether-a-given-item-is-the-left-or-right) endpoint.

Developers and integrators may extend the available projections for use with a DSpace instance by adding
a new uniquely-named `Projection` as a Java Spring `@Component`, and ensuring it is deployed (e.g. as a jar)
within the DSpace server webapp.

Upon successful component discovery, the projection will automatically become available for use with all
REST API endpoints using the same syntax as the standard projections (`?projection=name`).

See [the Projection javadocs](https://github.com/DSpace/DSpace/blob/master/dspace-server-webapp/src/main/java/org/dspace/app/rest/projection/Projection.java)
for more information.

### HAL link impact of property-based projections

Because property-based projections impact the JSON contents of the given record, the self-link will be impacted.  
Any other links HAL links to this record will also be impacted.

If there's a projection which adds a property `test` to the `item` if the `item` was requested using a custom projection,
the response cannot be cached identical to the request without this projection.  
This implies the HAL link to that item should contain the `projection` parameter, to clarify a cached version without that `projection` parameter is not valid.  
This also implies the self link of that item should contain the same `projection` parameter.  
If the projection used additional parameters to identify how it should be applied, those additional parameters should also be part of the HAL link.
