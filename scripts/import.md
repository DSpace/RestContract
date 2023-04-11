# Import script
<!-- TOC -->
* [Import script](#import-script)
  * [What it does?](#what-it-does)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion](#conclusion)
* [What is SAF?](#what-is-saf)
  * [DSpace Simple Archive Format](#dspace-simple-archive-format)
  * [A visual representation of a SAF:](#a-visual-representation-of-a-saf)
<!-- TOC -->
## What it does?

This script imports items or collections from Simple Archive Format (SAF).

It takes several parameters to customize the import, such as the action to perform, the zip file to import, the
destination collection, whether to send the submission through the collection's workflow, and whether to validate the
import.

## When we use it?

The Batch Import from Simple Archive Format (SAF) script is used to import items into DSpace from a zip file in SAF
format. The script provides a variety of parameters to control how the import is executed, including options to add,
replace, or delete items, map items using a mapfile, send submissions through a collection's workflow, and more.

## How it Works?

The Batch Import from SAF script reads a zip file in SAF format and extracts the items contained within. The script then
processes the items according to the parameters provided by the user, such as adding them to a collection, replacing
them in a mapfile, or deleting them.

## Parameters

The script takes the following parameters:

| Parameter                        | Description                                                                                                                                                                            |
|----------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-a`, <br>`--add`                | This parameter specifies that items in the SAF should be added to DSpace.                                                                                                              |
| `-r`, <br>`--replace`            | This parameter specifies that items in the SAF should replace items in the mapfile in DSpace.                                                                                          |
| `-d`, <br>`--delete`             | This parameter specifies that items listed in the mapfile should be deleted from DSpace.                                                                                               |
| `-z`, <br>`--zip`                | This parameter specifies the name of the zip file containing the SAF.                                                                                                                  |
| `-c`, <br>`--collection`         | This parameter specifies the destination collection(s) in DSpace where the items will be added, replaced, or deleted. It can be either the Handle or database ID of the collection(s). |
| `-m`, <br>`--mapfile`            | This parameter specifies the name of the mapfile that maps items in the SAF to items in DSpace.                                                                                        |
| `-w`, <br>`--workflow`           | This parameter specifies whether to send the submission through the destination collection's workflow. If specified, the submission will be sent through the workflow.                 |
| `-n`, <br>`--notify`             | This parameter specifies whether to send notification emails when sending submissions through the workflow. If specified, notification emails will be sent.                            |
| `-v`, <br>`--validate`           | This parameter specifies whether to do a test run and not actually import items. If specified, the import will be validated.                                                           |
| `-x`, <br>`--exclude-bitstreams` | This parameter specifies whether to exclude content bitstreams from the import. If specified, bitstreams will not be loaded or expected. Optional.                                     |
| `-p`, <br>`--template`           | This parameter specifies whether to apply a template to the items being imported. If specified, a template will be applied.                                                            |
| `-R`, <br>`--resume`             | This parameter specifies whether to resume a failed import and add only. If specified, the import will resume from where it failed.                                                    |
| `-q`, <br>`--quiet`              | This parameter specifies whether to suppress metadata display. If specified, metadata will not be displayed.                                                                           |
| `-h`, <br>`--help`               | This parameter displays help information for the script.                                                                                                                               |

The mapfile parameter allows users to map items to existing items in the DSpace repository. This can be useful for
replacing or updating existing items, or for creating relationships between items.

The workflow parameter allows users to send items through a collection's workflow. This can be useful for ensuring that
items meet certain criteria or are reviewed before being added to the repository.

## Conclusion

The Batch Import from SAF script is a powerful tool for importing items into DSpace from a zip file in SAF format. The
script provides a wide range of parameters to control how the import is executed, and can be used to add, replace, or
delete items, map items using a mapfile, send submissions through a collection's workflow, and more.


# What is SAF?

## DSpace Simple Archive Format

The basic concept behind the DSpace's Simple Archive Format is to create an archive, which is a directory containing one
subdirectory per item. Each item directory contains a file for the item's descriptive metadata, and the files that make
up the item.

## A visual representation of a SAF:

<details>
  <summary>archive_directory/</summary>

  <details>
    <summary>item_000/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>

  <details>
    <summary>item_001/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>

  <details>
    <summary>item_002/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>
  <details>
    <summary>item_003/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>

  <details>
    <summary>item_004/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>

  <details>
    <summary>item_005/</summary>

    * dublin_core.xml         -- qualified Dublin Core metadata for metadata fields belonging to the 'dc' schema.
    * metadata_[prefix].xml   -- metadata in another schema.  The prefix is the name of the schema as registered with the metadata registry.
    * contents                -- text file containing one line per filename.
    * collections             -- (Optional) text file that contains the handles of the collections the item will belong to. Each handle in a row. Collection in first line will be the owning collection.
    * relationships           -- (Optional) If importing Entities, you can specify one or more relationships to create on import
    * file_1.doc              -- files to be added as bitstreams to the item.
    * file_2.pdf
  </details>
</details>


In the above format, the directory archive_directory is the top-level directory, and each item is a subdirectory within
it.

Each item subdirectory contains the following files and directories:

`dublin_core.xml`:
This is a qualified Dublin Core metadata file that contains metadata fields belonging to the 'dc' schema.

`metadata_[prefix].xml`: This is a metadata file that contains metadata in another schema. The prefix is the name of the
schema as registered with the metadata registry.

`contents`: This is a text file that contains one line per filename.

`collections`: (Optional) This is a text file that contains the handles of the collections the item will belong to. Each
handle is in a row, and the collection in the first line will be the owning collection.

`relationships`: (Optional) If importing entities, you can specify one or more relationships to create on import.

`file_1.doc`, `file_2.pdf`, `etc.`: These are the files that make up the item and will be added as bitstreams to the
item.


