# export script
[Back to the script index](index.md)
<!-- TOC -->
* [export script](#export-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
* [What is SAF?](#what-is-saf)
  * [DSpace Simple Archive Format](#dspace-simple-archive-format)
  * [A visual representation of a SAF](#a-visual-representation-of-a-saf)
<!-- TOC -->
## What it does?

The export script can be used to export individual items or entire collections, and can be customized to exclude
bitstreams and metadata as needed.

## Who can use it?

This script can be used by any user that has administrative privileges over at least one community, collection or item limited to the resources that they admin.

## When we use it?

The export script is used to batch export items from DSpace to Simple Archive Format (SAF) for migration or backup
purposes.

[See below](#what-is-saf) for more information about the SAF export format.


## How it works?

The export script works by querying the DSpace database to identify the item or collection to be exported. Once the item
or collection has been identified, the script creates a Simple Archive Format (SAF) file containing the item's metadata
and bitstreams (if applicable).

## Parameters

It takes several parameters to customize the export, such as the type of object to export, the ID or handle of the
object, the sequence number to begin exporting items with, whether to export for migration, and whether to exclude
bitstreams.

The following parameters are available for the export script:

| Parameter                         | Description                                                                              |
|-----------------------------------|------------------------------------------------------------------------------------------|
| `-t`, <br/>`--type <value>`       | The type of item to export. Valid values include: COLLECTION or ITEM.                    |
| `-i`, <br/>`--id <value>`         | The ID or handle of the thing to export.                                                 |
| `-n`, <br/>`--number <value>`     | The sequence number to begin exporting items with.                                       |
| `-m`, <br/>`--migrate`            | Export for migration (remove handle and metadata that will be re-created in new system). |
| `-x`, <br/>`--exclude-bitstreams` | Do not export bitstreams.                                                                |
| `-h`, <br/>`--help`               | Show help.                                                                               |

The type parameter specifies whether to export a collection or an item. If exporting a collection, the script will
export all items within the collection. The ID parameter specifies the ID or handle of the item or collection to be
exported.

The number parameter is used to specify the sequence number to begin exporting items with. This parameter is useful when
exporting large collections, as it allows you to export items in batches rather than exporting the entire collection at
once.

The migrate parameter is used to export items for migration. When exporting items for migration, the script will remove
the handle and metadata that will be re-created in the new system.

The exclude-bitstreams parameter is used to exclude bitstreams from the export. This parameter can be useful when
exporting large collections that contain many large files, as it can reduce the size of the resulting SAF file.

## Conclusion/QuickGuide:

The export script is a useful tool for batch exporting items from DSpace to Simple Archive Format (SAF). By default, the
script exports both metadata and bitstreams, but can be customized to exclude bitstreams and metadata as needed. If you
need to migrate your DSpace repository to a new system, the export script can be used to export items in a format that
can be easily imported into the new system.

# What is SAF?

## DSpace Simple Archive Format

The basic concept behind the DSpace's Simple Archive Format is to create an archive, which is a directory containing one
subdirectory per item. Each item directory contains a file for the item's descriptive metadata, and the files that make
up the item.

For more information on the Simple Archive Format, see the [DSpace Documentation](https://wiki.lyrasis.org/display/DSDOC7x/Importing+and+Exporting+Items+via+Simple+Archive+Format).

## A visual representation of a SAF

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





