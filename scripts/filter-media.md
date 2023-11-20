# filter-media script
[Back to the script index](index.md)
<!-- TOC -->
* [filter-media script](#filter-media-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

This script processes bitstreams (files) that are added to DSpace.  It can perform various actions on files, which include extracting full text from files (to allow for searching within files) and creation of file thumbnails.

For more information,
see the [Media Filter documentation](https://wiki.lyrasis.org/display/DSDOC7x/Mediafilters+for+Transforming+DSpace+Content)

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The Filter-Media script is used to extract full text from documents and create thumbnails in DSpace.


## How it Works?

The filter-media script processes bitstreams (files) that are added to DSpace, performing various activities on those files.  For instance, by default, it will extract the full text from files to make them full text searchable.  It will also create thumbnails.

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

The filter-media script is a powerful tool for extracting full text and creating thumbnails in DSpace. By default, the
script will process all bitstreams in DSpace and run all available Media Filter plugins. However, by using the available
parameters, you can customize the script to process only specific bitstreams or to use only certain Media Filter
plugins. This can help to optimize the performance of the script and ensure that the desired output is generated.
