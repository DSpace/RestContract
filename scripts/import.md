# import script
[Back to the script index](index.md)
<!-- TOC -->
* [import script](#import-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide](#conclusionquickguide)
<!-- TOC -->
## What it does?

This script imports items or collections from Simple Archive Format (SAF).

See [what is SAF](./export.md#what-is-saf)? For more information about the SAF export format.

It takes several parameters to customize the import, such as the action to perform, the zip file to import, the
destination collection, whether to send the submission through the collection's workflow, and whether to validate the
import.

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The import script is used to import items into DSpace from a zip file in SAF
format. The script provides a variety of parameters to control how the import is executed, including options to add,
replace, or delete items, map items using a mapfile, send submissions through a collection's workflow, and more.

## How it Works?

The import script reads a zip file in SAF format and extracts the items contained within. The script then
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

## Conclusion/QuickGuide

The import script is a powerful tool for importing items into DSpace from a zip file in SAF format. The
script provides a wide range of parameters to control how the import is executed, and can be used to add, replace, or
delete items, map items using a mapfile, send submissions through a collection's workflow, and more.



