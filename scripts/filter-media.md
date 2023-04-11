# Filter-Media Script
<!-- TOC -->
* [Filter-Media Script](#filter-media-script)
  * [What it does?](#what-it-does)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The script processes bitstreams that are added to DSpace, and extracts metadata and full text from the files. This information is then used to create thumbnails.

## When we use it?

The Filter-Media script is used to extract full text from documents and create thumbnails in DSpace.

## How it Works?

The Filter-Media script processes bitstreams that are added to DSpace, and extracts metadata and full text from the files. This information is then used to create thumbnails and make the full text searchable in DSpace.

The script can be run with a variety of parameters to control which bitstreams are processed and which Media Filter plugins are used.

By default, the script will process all bitstreams in DSpace and run all Media Filter plugins listed in the 'filter.plugins' configuration property in dspace.cfg.

## Parameters

| Parameter                         | Description                                                                                                                                                                                                   |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-v`, <br/>`--verbose`            | Print all extracted text and other details to STDOUT.                                                                                                                                                         |
| `-q`, <br/>`--quiet`              | Do not print anything except in the event of errors.                                                                                                                                                          |
| `-f`, <br/>`--force`              | Force all bitstreams to be processed.                                                                                                                                                                         |
| `-i`, <br/>`--identifier <value>` | ONLY process bitstreams belonging to the specified identifier.                                                                                                                                                |
| `-m`, <br/>`--maximum <value>`    | Process no more than the specified number of items.                                                                                                                                                           |
| `-p`, <br/>`--plugins <value>`    | ONLY run the specified Media Filter plugin(s) listed from 'filter.plugins' in dspace.cfg. Separate multiple plugins with a comma (,) (e.g. MediaFilterManager -p "Word Text Extractor","PDF Text Extractor"). |
| `-s`, <br/>`--skip <value>`       | SKIP the bitstreams belonging to the specified identifier. Separate multiple identifiers with a comma (,) (e.g. MediaFilterManager -s 123456789/34,123456789/323).                                            |
| `-h`, <br/>`--help`               | Display help information for the script.                                                                                                                                                                      |

## Conclusion/QuickGuide:

The Filter-Media script is a powerful tool for extracting full text and creating thumbnails in DSpace. By default, the
script will process all bitstreams in DSpace and run all available Media Filter plugins. However, by using the available
parameters, you can customize the script to process only specific bitstreams or to use only certain Media Filter
plugins. This can help to optimize the performance of the script and ensure that the desired output is generated.
