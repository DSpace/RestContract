# solr-database-resync script
[Back to the script index](index.md)
<!-- TOC -->
* [solr-database-resync script](#solr-database-resync-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

This script updates the database status of items in Solr.

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The script is typically used when there is a mismatch between the database and the Solr index, which can happen if
changes are made to the database outside of DSpace, or if there are errors during indexing. The status in the Solr index
may become out of sync with the status in the database, causing issues with search results.

## How it Works?

When an item is added to DSpace, its metadata is stored in the database, and an index is created in Solr. The solr index
includes a "status" field, which indicates whether the item is "in archive", "withdrawn", or "discoverable". If changes
are made to the database outside of DSpace, or if there are errors during indexing, the status in the Solr index may
become out of sync with the status in the database.

The solr-database-resync script updates the database status of items in Solr based on the current index state. The
script retrieves all items from the Solr index, along with their status. It then queries the database to retrieve the
current status of each item and updates the status in the database if necessary. Finally, it triggers a reindex of each
item to ensure that the Solr index is updated with the correct status.

## Parameters

The script takes no parameters.

| Parameter    | Description |
|--------------|-------------|
| no parameter |             |

## Conclusion/QuickGuide:

The solr-database-resync script is a powerful tool for ensuring that the database and Solr index are in sync. However,
it should be used with caution and only when necessary as it can modify the database status of a large number of items.
It is recommended to back up your database before running this script in case any issues arise. If you encounter issues
with search results or item statuses, this script may be a helpful tool to help diagnose and resolve the issue.
