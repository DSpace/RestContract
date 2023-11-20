# metadata-import script
[Back to the script index](index.md)
<!-- TOC -->
* [metadata-import script](#metadata-import-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The metadata-import script is designed to import metadata from a CSV file after batch editing in DSpace.

For more information,
see the [Batch Metadata Editing documentation](https://wiki.lyrasis.org/display/DSDOC7x/Batch+Metadata+Editing)

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

This script is useful when making large-scale changes to items in DSpace, allowing for easy updating of metadata in
bulk.



## How it Works?

The metadata-import script reads a CSV file containing metadata to import, and updates the relevant items in DSpace with
the new metadata. This can be useful after batch editing a large number of items.

## Parameters

| Parameter           | Description                                                                  |
|---------------------|------------------------------------------------------------------------------|
| `-f <file>`         | Source file. Specifies the CSV file containing the metadata to import.       |
| `-s`                | Silent operation. Doesn't request confirmation of changes. Use with caution. |
| `-w`                | Workflow. When adding new items, use collection workflow.                    |
| `-n`                | Notify. When adding new items using a workflow, send notification emails.    |
| `-v`                | Validate only. Just validate the CSV, don't run the import.                  |
| `-t`                | Template. When adding new items, use the collection template (if it exists). |
| `-h`, <br/>`--help` | Print the help message.                                                      |

## Conclusion/QuickGuide:

The metadata-import script is a useful tool for importing metadata after batch editing in DSpace. When running the
script, be sure to use caution when running in silent mode, and always validate CSV files before importing metadata into
DSpace. With this guide, you should now be able to use the script effectively by specifying any relevant parameters.
