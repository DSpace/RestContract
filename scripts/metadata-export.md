# Metadata-Export script
<!-- TOC -->
* [Metadata-Export script](#metadata-export-script)
  * [What it does?](#what-it-does)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The Metadata-Export script exports the metadata for a specific item, collection, or community in CSV format.

## When we use it?

The Metadata-Export script is used to export metadata for batch editing in DSpace. Users can export metadata for
specific items, collections, or communities, and then modify the metadata in a spreadsheet or text editor. Once the
metadata has been edited, it can be re-imported using the Metadata-Import script.

## How it Works?

The Metadata-Export script queries the DSpace database to retrieve metadata for the specified item, collection, or
community. The metadata is then exported to a file in CSV (comma-separated values) format. By default, the exported
metadata only includes fields that are normally edited by users, such as titles, authors, and dates. If the `-a`
or `--all` parameter is used, all metadata fields will be included in the export, including fields that are not normally
changed, such as provenance.

## Parameters

The following table summarizes the available parameters for the Metadata-Export script:

| Parameter                 | Description                                                                    |
|---------------------------|--------------------------------------------------------------------------------|
| `-i`, <br/>`--id <value>` | The ID or handle of the item, collection, or community to export metadata for. |
| `-a`, <br/>`--all`        | Include all metadata fields that are not normally changed (e.g. provenance).   |
| `-h`, <br/>`--help`       | Display help information.                                                      |

## Conclusion/QuickGuide:

The Metadata-Export script is a useful tool for batch editing metadata in DSpace. By exporting metadata to a spreadsheet
or text editor, users can make changes to multiple items, collections, or communities at once. Once the metadata has
been modified, it can be re-imported using the Metadata-Import script.
