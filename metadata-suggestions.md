# Metadata suggestions from live import
[Back to the index](README.md)

**Contents:**

- [Introduction](#introduction)
- [Main Endpoint](#main-endpoint)
- [Single suggestion endpoint](#single-suggestion-endpoint)
- [Linked entities](#linked-entities)
	- [Live import entries](#live-import-entries)
	- [Single entry](#single-entry)
	- [Differences for a single entry](#differences-for-a-single-entry)
- [Changes suggested from the live import](#changes-suggested-from-the-live-import)
	- [Introduction](#introduction)
	- [Adding metadata](#adding-metadata)
	- [Replacing metadata](#replacing-metadata)
	- [Replacing name variants](#replacing-name-variants)
	- [Adding relationships](#adding-relationships)

## Introduction

This contract allows for suggesting metadata changes to an in-submission or in-workflow item.
The user can choose a live import source to use for suggesting the metadata (e.g. PubMed for a medical publication, ORCID for an author, a RIS file for uploaded metadata).
The live import source will suggest matches, and will suggest metadata changes if the match is accepted by the user.
A full metadata description of the data from the live import source is displayed in combination to the suggested changes to the current item.

## Main Endpoint
**/api/integration/metadata-suggestions**

Provide access to the configured live import sources which can suggest metadata.
It returns the list of existent live import sources.

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

Provide detailed information about a specific live import source. The JSON response document is as follow
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
* query-based: if the live import source uses a query to suggest information to import
* file-based: if the live import source uses a file to suggest information to import
* metadata-based: if the live import source uses the current item metadata to suggest information to import

Exposed links:
* entries: the list of values managed by the live import source

## Linked entities
### Live import entries
**/api/integration/metadata-suggestions/<:suggestion-name>/entries**

It returns the filtered entries managed by the live import source, see below

The supported parameters are:
* page, size [see pagination](README.md#Pagination) if supported by the live import source
* query: the terms, keywords or prefix to search. Applicable for sources where "query-based" is true
* bitstream: the bitstream ID to process. Applicable for sources where "file-based" is true
* use-metadata: enable metadata based search (true or false, defaults to false). Applicable for sources where "metadata-based" is true
* workspaceitem: the current workspace item ID (mutually exclusive with workflowitem)
* workflowitem: the current workflow item ID (mutually exclusive with workspaceitem)

It returns the entries in the live import source matching the query, bitstream or item metadata

sample for a live import source /api/integration/metadata-suggestions/orcid/entries?query=Smith&size=2
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
          "differences": {
            "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValueDifferences/0000-0002-4271-0436"
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
         "differences": {
           "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValueDifferences/0000-0003-3681-2038"
         }
        }
      }
    ]
  }
}
```

The differences are explained below, and won't be embedded in this endpoint.

### Single entry
**/api/integration/metadata-suggestions/<:suggestion-name>/entryValues/<:entry-id>**  

It returns the data from one entry in a live import source

sample for a live import source /api/integration/metadata-suggestions/orcid/entryValues/0000-0002-4271-0436
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
    "differences": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValueDifferences/0000-0002-4271-0436"
    }
  }
}
```

The differences are explained below

### Differences for a single entry
**/api/integration/metadata-suggestions/<:suggestion-name>/entryValueDifferences/<:entry-id>**  

Parameters (workspaceitem or workflowitem is mandatory):
* workspaceitem: the current workspace item ID (mutually exclusive with workflowitem)
* workflowitem: the current workflow item ID (mutually exclusive with workspaceitem)

It returns the suggested metadata changes from one entry to be applied to the given item

sample for a live import source /api/integration/metadata-suggestions/orcid/entryValueDifferences/0000-0002-4271-0436?workspaceitem=123
```json
{
  "differences" : {
    "dc.identifier.orcid" : {
      "currentvalues" : [],
      "suggestions" : [
        {
          "operations": [ "add/metadata/dc.identifier.orcid/-" ],
          "newvalue": "0000-0002-4271-0436"
        }
      ]
    },
    "dc.identifier.uri" : {
      "currentvalues" : [ "https://profiles.utsouthwestern.edu/profile/16780/dean-smith.html" ],
      "suggestions" : [
        {
          "operations": [ "add/metadata/dc.identifier.uri/-", "replace/metadata/dc.identifier.uri/0" ],
          "newvalue": "https://orcid.org/0000-0002-4271-0436"
        }
      ]
    }
  },
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/orcid/entryValueDifferences/0000-0002-4271-0436?workspaceitem=123"
    }
  }
}
```

sample for a live import source /api/integration/metadata-suggestions/pubmed/entryValueDifferences/31383667?workspaceitem=456
```json
{
  "differences" : {
    "dc.contributor.author" : {
      "currentvalues" : [ "D. Pollard", "C. E. Wylie[virtual::1373]" ],
      "suggestions" : [
        {
          "operations": [ "replace/metadata/dc.contributor.author/0", "add/metadata/dc.contributor.author/-" ],
          "newvalue": "Pollard, D"
        },
        {
          "operations": [ "namevariant/metadata/dc.contributor.author/1", "add/metadata/dc.contributor.author/-" ],
          "newvalue": "Wylie, C E"
        },
        {
          "operations": [ "add-relationship/metadata/dc.contributor.author/-" ],
          "newvalue": "Newton, J R",
          "item": "1f8b3dd7-f5ae-4153-bf17-808d3a5379ac",
          "relationship-type" : 1
        }
    ]
    },
    "dc.date.issued" : {
      "currentvalues": [ "2019" ],
      "suggestions": [
        {
          "operations": [ "replace/metadata/dc.date.issued/0" ],
          "newvalue": "2019-09-23"
        }
      ]
    },
    "dc.subject" : {
      "currentvalues" : [],
      "suggestions": [
        {
          "operations": [ "add/metadata/dc.subject/-" ],
          "newvalue": "DNA replication inhibitors"
        },
        {
          "operations": [ "add/metadata/dc.subject/-" ],
          "newvalue": "Mycobacterium"
        }
      ]
    }
  },
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/integration/metadata-suggestions/pubmed/entryValueDifferences/31383667?workspaceitem=456"
    }
  }
}
```

The suggested changes are based on the current item:
* It takes the current metadata of the item into account (don't suggest to add a title which is already present)
* It takes the submission forms into account (don't suggest to change metadata fields which are not editable based on input forms, only one value for a non-repeatable field)
* It suggests operations to apply to the item. The first operation is considered the most relevant by REST (but the user can choose another operation)

## Changes suggested from the live import
### Introduction

The live import can suggest metadata changes.
The user should be given the choice to apply or ignore these metadata changes.

The suggestions will be presented in a format similar to the [Metadata Patch](metadata-patch.md)
to allow the user-interface to easily display and apply the suggested changes.

The suggestions include per metadata value:
* The current values present in the item
* Per suggested change, one or more operations (the first operation is assumed to be the preferred action)
  * "add" is used to suggest adding the value at the end
  * "replace/<:place>" is used to suggest replacing the value at <:place> with the new value
  * "namevariant/<:place>" is used to suggest replacing the name variant of the relationship at <:place> with the new value
  * "add-relationship" is used to suggest creating a new relationship
* The new value for the suggested change

The examples in each section below build on each other, assuming an initial metadata state of:

```json
{
  "metadata": {
    "dc.contributor.author": [
      { "value": "D. Pollard", "language": null, "authority": null, "confidence": -1 },
      { "value": "C. E. Wylie", "language": null, "authority": "virtual::1373", "confidence": 600 }
    ],
    "dc.date.issued": [
      { "value": "2019", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```

The user interface can display the suggested metadata changes, and request the user to accept or reject the changes.
If accepted, the user interface can perform a PATCH based on the suggested metadata changes to apply the suggestions.

### Adding metadata

With the `add` operation, the live import suggests to add metadata new values.

Example response, suggestion the addition of three values:

```json
{
  "dc.subject" : {
    "currentvalues" : [],
    "suggestions": [
      {
        "operations": [ "add/metadata/dc.subject/-" ],
        "newvalue": "DNA replication inhibitors"
      },
      {
        "operations": [ "add/metadata/dc.subject/-" ],
        "newvalue": "Mycobacterium"
      }
    ]
  }
}
```

If this patch is applied hereafter, the new metadata state is:

```json
{
  "metadata": {
    "dc.contributor.author": [
      { "value": "D. Pollard", "language": null, "authority": null, "confidence": -1 },
      { "value": "C. E. Wylie", "language": null, "authority": "virtual::1373", "confidence": 600 }
    ],
    "dc.date.issued": [
      { "value": "2019", "language": null, "authority": null, "confidence": -1 }
    ],
    "dc.subject": [
      { "value": "DNA replication inhibitors", "language": null, "authority": null, "confidence": -1 },
      { "value": "Mycobacterium", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```

### Replacing metadata

With `replace`, the live import suggests to overwrite a single existing metadata value:
* `replace/<:place>` is used to suggest replacing the value at <:place> with the new value

Example response, replacing the the first `dc.date.issued`:

```json
{
    "dc.date.issued" : {
      "currentvalues": [ "2019" ],
      "suggestions": [
        {
          "operations": [ "replace/metadata/dc.date.issued/0" ],
          "newvalue": "2019-09-23"
        }
      ]
    }
}
```

If this patch is applied hereafter, the new metadata state is:

```json
{
  "metadata": {
    "dc.contributor.author": [
      { "value": "D. Pollard", "language": null, "authority": null, "confidence": -1 },
      { "value": "C. E. Wylie", "language": null, "authority": "virtual::1373", "confidence": 600 }
    ],
    "dc.date.issued": [
      { "value": "2019-09-23", "language": null, "authority": null, "confidence": -1 }
    ],
    "dc.subject": [
      { "value": "DNA replication inhibitors", "language": null, "authority": null, "confidence": -1 },
      { "value": "Mycobacterium", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```

### Replacing name variants

With `namevariant`, the live import suggests to overwrite a single name variant:
* `namevariant/<:place>` is used to suggest replacing the name variant of the relationship at <:place> with the new value

Example response, replacing the the second `dc.contributor.author` name variant:

```json
{
    "dc.contributor.author" : {
      "currentvalues" : [ "D. Pollard", "C. E. Wylie[virtual::1373]" ],
      "suggestions" : [
        {
          "operations": [ "namevariant/metadata/dc.contributor.author/1", "add/metadata/dc.contributor.author/-" ],
          "newvalue": "Wylie, C E"
        }
      ]
    }
}
```

The above example includes two possible operations: an option to replace the `namevariant`, and an option to add the `newvalue` as an additional author.
Assuming the client chooses the first operation (replace the namevariant), the new metadata state is:

```json
{
  "metadata": {
    "dc.contributor.author": [
      { "value": "D. Pollard", "language": null, "authority": null, "confidence": -1 },
      { "value": "Wylie, C E", "language": null, "authority": "virtual::1373", "confidence": 600 }
    ],
    "dc.date.issued": [
      { "value": "2019-09-23", "language": null, "authority": null, "confidence": -1 }
    ],
    "dc.subject": [
      { "value": "DNA replication inhibitors", "language": null, "authority": null, "confidence": -1 },
      { "value": "Mycobacterium", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```

### Adding relationships

With `add-relationship`, the live import suggests to add a new relationship
* `add-relationship` is used to suggest creating a new relationship

Example response, adding a `dc.contributor.author` relationship:

```json
{
    "dc.contributor.author" : {
      "currentvalues" : [ "D. Pollard", "C. E. Wylie[virtual::1373]" ],
      "suggestions" : [
        {
          "operations": [ "add-relationship/metadata/dc.contributor.author/-" ],
          "newvalue": "Newton, J R",
          "item": "1f8b3dd7-f5ae-4153-bf17-808d3a5379ac",
          "relationship-type" : 1
        }
      ]
    }
}
```

If this patch is applied hereafter, the new metadata state is:

```json
{
  "metadata": {
    "dc.contributor.author": [
      { "value": "D. Pollard", "language": null, "authority": null, "confidence": -1 },
      { "value": "Wylie, C E", "language": null, "authority": "virtual::1373", "confidence": 600 },
      { "value": "Newton, J R", "language": null, "authority": "virtual::1375", "confidence": 600 }
    ],
    "dc.date.issued": [
      { "value": "2019-09-23", "language": null, "authority": null, "confidence": -1 }
    ],
    "dc.subject": [
      { "value": "DNA replication inhibitors", "language": null, "authority": null, "confidence": -1 },
      { "value": "Mycobacterium", "language": null, "authority": null, "confidence": -1 }
    ]
  }
}
```
