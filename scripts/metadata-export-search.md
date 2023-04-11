# Metadata-Export-Search Script
<!-- TOC -->
* [Metadata-Export-Search Script](#metadata-export-search-script)
  * [What it does?](#what-it-does)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The Metadata-Export-Search script exports metadata from a discovery search in DSpace in CSV format.

## When we use it?

The Metadata-Export-Search script is used to export metadata from a discovery search in DSpace. This script can be
useful for creating reports or generating metadata exports based on specific search criteria.

## How it Works?

The script works by using the DSpace Discovery search API to execute a search based on the specified search string and
search parameters. It then exports metadata from the search results using the Simple Archive Format (SAF) format, which
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

The Metadata-Export-Search script is a useful tool for exporting metadata from a discovery search in DSpace. By
specifying the search string and search parameters, users can export metadata from a specific subset of records that
match their search criteria. The script uses the CSV format, which is a standard format for
exporting metadata from DSpace, making it easy to import the metadata into other systems or analyze the metadata using
other tools.
