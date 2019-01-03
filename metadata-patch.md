# Modifying metadata via Patch
[Back to the index](README.md)

**Contents:**

* [Introduction](#introduction)
* [Adding metadata](#adding-metadata)
* [Removing metadata](#removing-metadata)
* [Replacing metadata](#replacing-metadata)
* [Ordering metadata](#ordering-metadata)

## Introduction

A user with write permission on a DSpace object may change any of its metadata
using an HTTP PATCH request. All DSpace object types (Bitstreams, Items, Collections, etc.)
support metadata changes in roughly the same way.

This guide is focused on metadata-only changes and omits non-metadata parts of the object's
json representation for brevity. However, you should be aware that PATCH requests operate
against the whole object, and may therefore modify other mutable attributes in the same
request.

For additional information about PATCH support in DSpace, including request format and response
status code details, see [General rules for the Patch operation](patch.md)

The examples in each section below build on each other, assuming an initial metadata state of:

```json
{
  "metadata": {
    "dc.title": [
      { "value": "Initial Title", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```

## Adding metadata

With the `add` operation, you can add any number of metadata values in a single request.

Note:

* When no values exist yet for a metadata key, you must add the key and value array with one
  or more values.
* When values already exist for a metadata key, it is necessary to specify the new value's
  position at the end of the `path` (`/0` means first position, `/-` means last).
* When creating new metadata values, if unspecified, the `language` and `authority` properties
  default to `null`, and `confidence` defaults to `-1`.

Example request, adding three values:

```json
[
  {
    "op": "add",
    "path": "/metadata/dc.description",
    "value": [ { "value": "Some description" } ]
  },
  {
    "op": "add",
    "path": "/metadata/dc.title/0",
    "value": { "value": "Zeroth Title" }
  },
  {
    "op": "add",
    "path": "/metadata/dc.title/-",
    "value": { "value": "Final Title", "language": "en_US" }
  }
]
```

New metadata state:

```json
{
  "metadata": {
    "dc.description": [
      { "value": "Some description", "language": null, "authority": null, "confidence": -1 }
    ],
    "dc.title": [
      { "value": "Zeroth Title", "language": null, "authority": null, "confidence": -1 },
      { "value": "Initial Title", "language": null, "authority": null, "confidence": -1 },
      { "value": "Final Title", "language": "en_US", "authority": null, "confidence": -1 }
    ]
  }
}
```

## Removing metadata

With the `remove` operation, you can delete:

* All metadata with a given key (e.g. `path="/metadata/dc.description"`)
* A single metadata value with a known position (e.g. `path="/metadata/dc.title/0"`)

When only one value is present for a particular key, it is functionally equivalent to
specify the path to the key or the value.

Example request, removing two values:

```json
[
  {
    "op": "remove",
    "path": "/metadata/dc.description"
  },
  {
    "op": "remove",
    "path": "/metadata/dc.title/0"
  }
]
```

New metadata state:

```json
{
  "metadata": {
    "dc.title": [
      { "value": "Initial Title", "language": null, "authority": null, "confidence": -1 },
      { "value": "Final Title", "language": "en_US", "authority": null, "confidence": -1 }
    ]
  }
}
```

## Replacing metadata

With `replace`, you can overwrite any of the following within a single operation
of a PATCH request:

* The entire set of all object metadata (`path="/metadata"`)
* All metadata values for an existing key (e.g. `path="/metadata/dc.title"`)
* A single existing metadata value (e.g. `path="/metadata/dc.title/0"`)
* A single property of an existing value (e.g. `path="/metadata/dc.title/0/language"`)

Example request, replacing the `value` and `language` of the first `dc.title`:

```json
[
  {
    "op": "replace",
    "path": "/metadata/dc.title/0/value",
    "value": "最後のタイトル"
  },
  {
    "op": "replace",
    "path": "/metadata/dc.title/0/language",
    "value": "ja_JP"
  }
]
```

New metadata state:

```json
{
  "metadata": {
    "dc.title": [
      { "value": "最後のタイトル", "language": "ja_JP", "authority": null, "confidence": -1 },
      { "value": "Final Title", "language": "en_US", "authority": null, "confidence": -1 }
    ]
  }
}
```

## Ordering metadata

Metadata may be re-ordered with the `move` operation.

Example request, moving the last `dc.title` value to the first position:

```json
[
  {
    "op": "move",
    "from": "/metadata/dc.title/1",
    "path": "/metadata/dc.title/0"
  }
]
```

New metadata state:

```json
{
  "metadata": {
    "dc.title": [
      { "value": "Final Title", "language": "en_US", "authority": null, "confidence": -1 },
      { "value": "最後のタイトル", "language": "ja_JP", "authority": null, "confidence": -1 }
    ]
  }
}
```
