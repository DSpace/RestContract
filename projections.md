# Projections
[REST Overview Documentation](README.md)

All `GET` requests returning a HAL document support an optional *projection* argument, specifying the name
of an alternative representation of the requested resource.

Projections may add to, modify, or omit information from the default representation of a resource.

When fulfilling a request, the active projection is applied to the primary resource being requested
as well as any embedded resources.

## Standard Projections

There are two standard projections available in all DSpace 7+ instances:

### Default Projection

The _default_ projection makes no changes to the normal representation of the resource and **excludes all
subresource embeds**, omitting altogether the `_embedded` section of the HAL representation.

This is the implicit projection for all individual resource endpoints, such as `/api/core/items/<:uuid>`,
if no projection is specified.

### Full Projection (`?projection=full`)

The _full_ projection includes all linked subresources also embedded in the response.

Since embeds may include other embedded resources, it is important to limit the number of embed levels
alloweds. Thus, only two levels of embeds will be returned at maximum when the _full_ projection is requested.

## Custom Projections

Developers and integrators may extend the available projections for use with a DSpace instance by adding
a new uniquely-named `Projection` as a Java Spring `@Component`, and ensuring it is deployed (e.g. as a jar)
within the DSpace server webapp.

Upon successful component discovery, the projection will automatically become available for use with all
REST API endpoints using the same syntax as the standard projections (`?projection=name`).

See [the Projection javadocs](https://github.com/DSpace/DSpace/blob/master/dspace-server-webapp/src/main/java/org/dspace/app/rest/projection/Projection.java)
for more information.
