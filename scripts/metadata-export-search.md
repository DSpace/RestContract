# metadata-export-search Script
[Back to the script index](index.md)
<!-- TOC -->
* [metadata-export-search Script](#metadata-export-search-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The metadata-export-search script exports metadata from a discovery search in DSpace in CSV format.

For more information,
see the [Batch Metadata Editing documentation](https://wiki.lyrasis.org/display/DSDOC7x/Batch+Metadata+Editing)

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The metadata-export-search script is used to export metadata from a discovery search in DSpace. This script can be
useful for creating reports or generating metadata exports based on specific search criteria.

This export can also be used as a starting point
to perform offline bulk changes to the record via the [Batch Metadata Editing feature](https://wiki.lyrasis.org/display/DSDOC7x/Batch+Metadata+Editing)

## How it Works?

The script works by using the DSpace Discovery search API to execute a search based on the specified search string and
search parameters. It then exports metadata from the search results using CSV format, which
is a standard format for exporting metadata from DSpace.

## Parameters

| Parameter                    | Description                                                                                                                                                                                                                                                 |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-q`, <br/>`--query`         | The discovery search string to be used to match records. Not URL encoded.                                                                                                                                                                                   |
| `-s`, <br/>`--scope`         | UUID of a specific DSpace container (site, community, or collection) to which the search has to be limited.                                                                                                                                                 |
| `-c`, <br/>`--configuration` | The name of a Discovery configuration that should be used by this search.                                                                                                                                                                                   |
| `-f`, <br/>`--filter`        | Advanced search filter that has to be used to filter the result set, with syntax `<:filter-name>,<:filter-operator>=<:filter-value>`. Not URL encoded. For example `author,authority=5df05073-3be7-410d-8166-e254369e4166` or `title,contains=sample text`. |
| `-h`, <br/>`--help`          | Display help information.                                                                                                                                                                                                                                   |

## Conclusion/QuickGuide:

The metadata-export-search script is a useful tool for exporting metadata from a discovery search in DSpace. By
specifying the search string and search parameters, users can export metadata from a specific subset of records that
match their search criteria. The script uses the CSV format, which is a standard format for
exporting metadata from DSpace, making it easy to import the metadata into other systems or analyze the metadata using
other tools.
