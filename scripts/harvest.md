# harvest script
[Back to the script index](index.md)
<!-- TOC -->
* [harvest script](#harvest-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters:](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

With this script, you can configure a collection to harvest its items from an external OAI-PMH instance. You can specify whether to harvest metadata only, or files as well (for external sites that support harvesting files).

For more information, see the [OAI documentation](https://wiki.lyrasis.org/display/DSDOC7x/OAI) 

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The script can be used to set up configurations related to a collection for harvesting its items,
run the harvest procedure, or manage the status of
harvested collections.

The harvest script can also be used to reset the status of harvested collections, purge collections, and re-import all
items in a collection.

## How it Works?

The harvest script uses the OAI-PMH protocol to harvest metadata from external collections and import it into DSpace.

When a collection is set up for harvesting, the script establishes a connection with the specified OAI-PMH server and
imports metadata for the specified set ID and metadata format. The harvested metadata is then imported into DSpace, and
items are created or updated as necessary.

## Parameters:

| Parameter                              | Description                                                                                                        |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `-p`, <br/>`--purge`                   | Deletes all items in the collection.                                                                               |
| `-r`, <br/>`--run`                     | Runs the standard harvest procedure.                                                                               |
| `-g`, <br/>`--ping`                    | Tests the OAI server and sets it up for harvesting.                                                                |
| `-s`, <br/>`--setup`                   | Sets the collection up for harvesting.                                                                             |
| `-S`, <br/>`--start`                   | Starts the harvest loop.                                                                                           |
| `-R`, <br/>`--reset`                   | Resets harvest status on all collections.                                                                          |
| `-P`, <br/>`--purgeCollections`        | Purges all harvestable collections.                                                                                |
| `-o`, <br/>`--reimport`                | ReImports all items in the collection. This is equivalent to using `-p` and then `-r`.                             |
| `-c`, <br/>`--collection <value>`      | The harvesting collection (handle or ID).                                                                          |
| `-t`, <br/>`--type <value>`            | The type of harvesting (0 for none).                                                                               |
| `-a`, <br/>`--address <value>`         | The address of the OAI-PMH server.                                                                                 |
| `-i`, <br/>`--oai_set_id <value>`      | The ID of the OAI-PMH set representing the harvested collection.                                                   |
| `-m`, <br/>`--metadata_format <value>` | The name of the desired metadata format for harvesting. This is resolved to namespace and crosswalk in dspace.cfg. |
| `-h`, <br/>`--help`                    | Shows help information.                                                                                            |

## Conclusion/QuickGuide:

The harvest script is a useful tool for managing the OAI-PMH harvesting of external collections in DSpace. It provides a
range of options for setting up collections, running the harvest procedure, and managing harvested collections. If you
need to import metadata from external collections into DSpace, the Harvest script may be a helpful tool to use.
