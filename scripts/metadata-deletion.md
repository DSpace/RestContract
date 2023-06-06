# metadata-deletion script
[Back to the script index](index.md)
<!-- TOC -->
* [metadata-deletion script](#metadata-deletion-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The metadata-deletion script is designed to remove all values of a specified metadata field from each item in DSpace.

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The metadata-deletion script is useful when you need to remove a specific metadata field from all items in DSpace.

## How it Works?

The metadata-deletion script works by querying the DSpace database to identify all items that contain the specified
metadata field, and then deleting all values of that field from each item. This process can take some time depending on
the size of your DSpace repository and the number of items that contain the specified metadata field.

It's important to note that the metadata-deletion script will permanently delete all values of the specified metadata
field from all items in DSpace. This action cannot be undone, so use this script with caution.

## Parameters

| Parameter                       | Description                                    |
|---------------------------------|------------------------------------------------|
| `-m`, <br/>`--metadata <value>` | The metadata field name.                       |
| `-l`, <br/>`--list`             | Lists the metadata fields that can be deleted. |

## Conclusion/QuickGuide:

The metadata-deletion script is a powerful tool for removing a specific metadata field from all items in DSpace. Before
using this script, make sure you have a backup of your DSpace repository in case you need to restore any deleted
metadata values.
