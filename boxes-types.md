# Layout Box Types
[Layout Tab Endpoints](tabs.md)

The following table list the boxTypes available out-of-box in DSpace 11

boxType | description
------------ | -------------
METADATA | the box is used to visualize metadata and files of the item 
RELATION | the box is used to visualize linked items retrieved via a discovery query
METRICS | the box is used to visualize one or more metrics about the object

## Metadata Configuration

```json
{
  "id": 1,
  "rows": [
    {
      "style": "row-style",
      "cells": [
        {
          "style": "cell-style",
          "fields": [
            {
              bitstream: {
                bundle: "ORIGINAL", 
                metadataField: "dc.type",
                metadataValue: "picture"
              },
              rendering: "thumbnail",
              fieldType: "bitstream",
              styleLabel: null,
              styleValue: null
            }
          ]
        },
        {
          "fields": [
            {
              metadata: "dc.title",
              label: "Title",
              fieldType: "metadata",
              styleLabel: null,
              styleValue: null
            },
            {
              metadata: "crisrp.name",
              label: "Name",
              fieldType: "metadata",
              styleLabel: null,
              styleValue: null
            }
          ]
        }
      ]
    },
    {
      "cells": [
        {
          "style": "cell-style",
          "fields": [
            {
              metadata: "dc.contibutor.author",
              label: "Authors",
              rendering: "browselink",
              fieldType: "metadata",
              styleLabel: null,
              styleValue: null
            }
          ]
        }
      ]
    }
  ]
}
```

It provides the configuration for a component that visualize a list of metadata according to specific rules

Attributes
* the *id* attribute has the same value that the id of the related box
* the *label* attribute is the i18n key for the field label to visualize
* the *rendering* attribute defines the component to use to visualize the field. Examples are browselink, longtext, identifier, date, etc. for metadata field and preview, thubmnail, etc. for bitstream field 
* the *styleLabel* attribute allows to set arbitrary css styles to the label
* the *styleValue* attribute allows to set arbitrary css styles to the metadata value
* the *fieldType" is one of metadata or bitstream a corresponding attribute will be present
* the *labelAsHeading* attribute indicates if the label must be showed as header of the value instead of inline
* the *valuesInline* attribute indicates if multiple values of the same metadata should be shown inline
* the *metadata* attribute is the canonical name of the metadata to use (eg dc.contributor.author, dc.title, etc.)
* the *bitstream* attribute is an object containing details to filter the bitstreams to use. It can be the name of the bundle to use and/or the value of specfic bitstream metadata

## Metrics Configuration

Provides detailed information about the metrics associated to the box

```json
{
  "id": 1,
  "numColumns": 2,
  "metrics": ["views", "downloads"]
}
```

It provides the configuration for a component that visualize a list of metrics.

## Relation Configuration


Provides detailed information about the relation included in the box

```json
{
  "id": 1,
  "discovery-configuration": "the-name-of-the-discovery-configuration"
}
```

the-name-of-the-discovery-configuration will match a discovery configuration where a facet named "cluster" 
will be eventually defined  to group the result by arbitrary defined criteria (facet query)