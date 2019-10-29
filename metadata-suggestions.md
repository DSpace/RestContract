# Metadata suggestions from live import
[Back to the index](README.md)

**Contents:**

- [Introduction](#introduction)
- [Main Endpoint](#main-endpoint)
- [Single suggestion endpoint](#single-suggestion-endpoint)
- [Linked entities](#linked-entities)
	- [External source entries](#external-source-entries)
	- [Single entry](#single-entry)
	- [Changes for a single entry](#changes-for-a-single-entry)
- [Changes suggested from the external source](#changes-suggested-from-the-external-source)
	- [Introduction](#introduction)
	- [Adding metadata](#adding-metadata)
	- [Replacing metadata](#replacing-metadata)

## Introduction

This contract allows for suggesting metadata changes to an in-submission or in-workflow item.
The user can choose an external source to use for suggesting the metadata (e.g. PubMed for a medical publication, ORCID for an author, a RIS file for uploaded metadata).
The external source will suggest matches, and will suggest metadata changes if the match is accepted by the user.
A full metadata description of the data from the external source is displayed in combination to the suggested changes to the current item.

TODO:
* start a new submission based on a external source record

## Main Endpoint
**/api/integration/metadata-suggestions**

Provide access to the configured external sources which can suggest metadata.
It returns the list of existent external sources.

Parameters (or should this be retrieved from the submission forms?):
* workspaceitem
* workflowitem

The list can differ per item.
A Person item can be limited to sources containing people such as orcid.
A Publication item can be limited to sources containing publications such as PubMed

Example:
```json
{
  "_embedded": {
    "metadata-suggestions": [
      {
        "id": "pubmed",
        "name": "pubmed",
        "type": "metadataSuggestion",
        "query-based": "true",
        "file-based": "false",
        "metadata-based": "true",
        "_links": {
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/pubmed/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/pubmed"
          }
        }
      },
      {
        "id": "orcid",
        "name": "orcid",
        "type": "metadataSuggestion",
        "query-based": "true",
        "file-based": "false",
        "metadata-based": "true",
        "_links": {
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid"
          }
        }
      },
      {
        "id": "ciencia",
        "name": "ciencia",
        "type": "metadataSuggestion",
        "query-based": "true",
        "file-based": "false",
        "metadata-based": "true",
        "_links": {
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/ciencia/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/ciencia"
          }
        }
      },
      {
        "id": "ris_data_loader",
        "name": "ris_data_loader",
        "type": "metadataSuggestion",
        "query-based": "false",
        "file-based": "true",
        "metadata-based": "false",
        "_links": {
          "entries": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/ris_data_loader/entries"
          },
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/ris_data_loader"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 3,
    "totalPages": 1,
    "number": 0
  }
}
```

## Single suggestion endpoint
**/api/integration/metadata-suggestions/<:suggestion-name>**

Provide detailed information about a specific external source. The JSON response document is as follow
```json
{
    "id": "pubmed",
    "name": "pubmed",
    "type": "metadataSuggestion",
    "query-based": "true",
    "file-based": "false",
    "metadata-based": "true",
    "_links": {
      "entries": {
        "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/pubmed/entries"
      },
      "self": {
        "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/pubmed"
      }
    }
}
```

Properties:
* query-based: if the external source uses a query to suggest information to import
* file-based: if the external source uses a file to suggest information to import
* metadata-based: if the external source uses the current item metadata to suggest information to import

Exposed links:
* entries: the list of values managed by the external source

## Linked entities
### External source entries
**/api/integration/metadata-suggestions/<:suggestion-name>/entries**

It returns the filtered entries managed by the external source, see below

The supported parameters are:
* page, size [see pagination](README.md#Pagination) if supported by the external source
* query: the terms, keywords or prefix to search. Applicable for sources where "query-based" is true
* bitstream: the bitstream ID to process. Applicable for sources where "file-based" is true
* use-metadata: enable metadata based search (true or false, defaults to false). Applicable for sources where "metadata-based" is true
* workspaceitem: the current workspace item ID (mutually exclusive with workflowitem)
* workflowitem: the current workflow item ID (mutually exclusive with workspaceitem)

It returns the entries in the external source matching the query, bitstream or item metadata

sample for an external source /api/integration/metadata-suggestions/orcid/entries?query=Smith&size=2
```json
{
  "_embedded": {
    "metadataSuggestionEntries": [
      {
        "id": "0000-0002-4271-0436",
        "display": "Smith, Dean",
        "value": "Smith, Dean",
        "metadata": {
            "dc.identifier.orcid": [
              {
                "value": "0000-0002-4271-0436",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "dc.identifier.uri": [
              {
                "value": "https://orcid.org/0000-0002-4271-0436",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.familyName": [
              {
                "value": "Smith",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.givenName": [
              {
                "value": "Dean",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ]
        },
        "type": "metadataSuggestionEntry",
        "_links": {
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436"
          },
          "changes": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436/changes"
          }
        }
      },
      {
        "id": "0000-0003-3681-2038",
        "display": "Smith, Charles",
        "value": "Smith, Charles",
        "metadata": {
            "dc.identifier.orcid": [
              {
                "value": "0000-0003-3681-2038",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "dc.identifier.uri": [
              {
                "value": "https://orcid.org/0000-0003-3681-2038",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.familyName": [
              {
                "value": "Smith",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ],
            "person.givenName": [
              {
                "value": "Charles",
                "language": null,
                "authority": null,
                "confidence": -1,
                "place": -1
              }
            ]
        },
        "type": "metadataSuggestionEntry",
        "_links": {
          "self": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0003-3681-2038"
          },
         "changes": {
           "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0003-3681-2038/changes"
         }
        }
      }
    ]
  }
}
```

The changes are explained below, and won't be embedded in this endpoint.

### Single entry
**/api/integration/metadata-suggestions/<:suggestion-name>/entryValues/<:entry-id>**  

Parameters:
* workspaceitem: the current workspace item ID (mutually exclusive with workflowitem)
* workflowitem: the current workflow item ID (mutually exclusive with workspaceitem)

It returns the data from one entry in an external source

sample for an external source /api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436
```json
{
  "id": "0000-0002-4271-0436",
  "display": "Smith, Dean",
  "value": "Smith, Dean",
  "type": "metadataSuggestionEntry",
  "metadataSuggestion": "orcid",
  "metadata": {
    "dc.identifier.orcid": [
      {
        "value": "0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "dc.identifier.uri": [
      {
        "value": "https://orcid.org/0000-0002-4271-0436",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "person.familyName": [
      {
        "value": "Smith",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ],
    "person.givenName": [
      {
        "value": "Dean",
        "language": null,
        "authority": null,
        "confidence": 0,
        "place": -1
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436"
    },
    "changes": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436/changes"
    }
  },
  "_embedded" : {
     "changes" : [
      {
        "op": "add",
        "path": "/metadata/dc.identifier.orcid",
        "value": [ { "value": "0000-0002-4271-0436" } ]
      },
      {
        "op": "add",
        "path": "/metadata/dc.identifier.uri/-",
        "value": [ { "value": "https://orcid.org/0000-0002-4271-0436" } ]
      }
     ]
  }
}
```

The changes embedded are the suggested metadata changes to be applied to the given item

### Changes for a single entry
**/api/integration/metadata-suggestions/<:suggestion-name>/entryValues/<:entry-id>/changes**  

Parameters:
* workspaceitem: the current workspace item ID (mutually exclusive with workflowitem)
* workflowitem: the current workflow item ID (mutually exclusive with workspaceitem)

It returns the suggested metadata changes from one entry to be applied to the given item

sample for an external source /api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436/changes
```json
{
  "changes" : [
    {
      "op": "add",
      "path": "/metadata/dc.identifier.orcid",
      "value": [ { "value": "0000-0002-4271-0436" } ]
    },
    {
      "op": "add",
      "path": "/metadata/dc.identifier.uri/-",
      "value": [ { "value": "https://orcid.org/0000-0002-4271-0436" } ]
    }
  ],
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436/changes"
    }
  }
}
```

The suggested changes are based on the current item:
* It takes the current metadata of the item into account (don't suggest to add a title which is already present)
* It takes the submission forms into account (don't suggest to change metadata fields which are not editable based on input forms, only one value for a non-repeatable field)

## Changes suggested from the external source
### Introduction

The live import can suggest metadata changes.
The user should be given the choice to apply or ignore these metadata changes.

The suggestions will be presented in a format compatible with the [Metadata Patch](metadata-patch.md)
to allow the user-interface to easily apply the suggested changes.

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

The user interface can display the suggested metadata changes, and request the user to accept or reject the changes.
If accepted, the user interface can perform a PATCH based on the suggested metadata changes to apply the suggestions.

### Adding metadata

With the `add` operation, the live import suggests to add metadata new values.

The `add` can also be used to effectively _replace_ all values for a given metadata key, by specifying
an already-present key as the `path`, and an array of replacement values as the `value`.
This use of the add operation to replace a list of values may be counter-intiutive, but it
is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)

> If the target location specifies an object member that does exist, that member's value is replaced.

Note:

* According to the [JSON Patch specification](https://tools.ietf.org/html/rfc6902), to initialize the first
  value for any array, the `add` operation must receive an array of values. Therefore, in the first operation
  below, since it is not possible to add a single value to the not yet initialized `/metadata/dc.description`
  array, we initialize it with an array with one value.
* When values already exist for a metadata key, as in the second operation below, it is necessary to specify
  the new value's position at the end of the `path` (`/0` means first position, `/-` means last).
* When creating new metadata values, if unspecified, the `language` and `authority` properties
  default to `null`, and `confidence` defaults to `-1`.

Example response, suggestion the addition of three values:

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

If this patch is applied hereafter, the new metadata state is:

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

### Replacing metadata

With `replace`, the live import suggests to overwrite any of the following within a single operation
of a PATCH request:

* The entire set of all object metadata (`path="/metadata"`)
* All metadata values for an existing key (e.g. `path="/metadata/dc.title"`)
* A single existing metadata value (e.g. `path="/metadata/dc.title/0"`)
* A single property of an existing value (e.g. `path="/metadata/dc.title/0/language"`)

Example response, replacing the `value` and `language` of the first `dc.title`:

```json
[
  {
    "op": "replace",
    "path": "/metadata/dc.title/0",
    "value": { "value": "最後のタイトル", "language": "ja_JP" }
  }
]
```

If this patch is applied hereafter, the new metadata state is:

```json
{
  "metadata": {
    "dc.title": [
      { "value": "最後のタイトル", "language": "ja_JP", "authority": null, "confidence": -1 },
      { "value": "Initial Title", "language": null, "authority": null, "confidence": -1 },
      { "value": "Final Title", "language": "en_US", "authority": null, "confidence": -1 }
    ]
  }
}
```
