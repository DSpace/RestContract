# Projections

## Questions
* How does an object convey its level of initialization (distinguishing empty from uninitialized)?
* Should an object know its own projection level?

## Projection = default
* Populate metadata

## Projection = context
Perhaps this should be the default.

* populate id, name, handle
* populate unique object attributes (such as discoverable, withdrawn)
* populate metadata
* populate link to parent object (breadcrumb projection)
* populate link to child objects (child projection)
 
## Projection = breadcrumb 
* populate id, name, handle
* populate parent object (breadcrumb projection)

## Projection = child
* populate id, nanme, handle

## Projection = descendant
* populate id, name, handle
* populate child objects (child projection)
