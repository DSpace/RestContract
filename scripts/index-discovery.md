# index-discovery script
[Back to the script index](index.md)
<!-- TOC -->
* [index-discovery script](#index-discovery-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters:](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The index-discovery script interacts with the Discovery Solr search index in DSpace. Depending on the parameters passed,
the script adds, removes, updates, or rebuilds the search index.

For more information regarding the Discovery infrastructure, see the [DSpace Documentation](https://wiki.lyrasis.org/display/DSDOC7x/Discovery).

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The index-discovery script is used to update the Discovery Solr search index in DSpace. The script allows for adding,
removing, updating, and rebuilding the search index.

## How it Works?

The index-discovery script updates the Discovery Solr search index in DSpace. The script queries the DSpace database to
identify the items, collections, or communities that need to be added, updated, or removed from the index, and then
performs the corresponding operations on the index.

## Parameters:

The following parameters are available for the index-discovery script:

| Parameter                     | Description                                                                                 |
|-------------------------------|---------------------------------------------------------------------------------------------|
| `-r`, <br/>`--remove <value>` | Remove an item, collection, or community from the index based on its handle.                |
| `-i`, <br/>`--index <value>`  | Add or update an item, collection, or community based on its handle or UUID.                |
| `-c`, <br/>`--clean`          | Clean the existing index, removing any documents that no longer exist in the database.      |
| `-d`, <br/>`--delete`         | Delete all records from the existing index.                                                 |
| `-b`, <br/>`--build`          | (Re)build the index, wiping out the current one if it exists.                               |
| `-s`, <br/>`--spellchecker`   | Rebuild the spellchecker. This option can be combined with `-b` and `-f`.                   |
| `-f`, <br/>`--force`          | If updating the existing index, force each handle to be reIndexed even if it is up-to-date. |
| `-h`, <br/>`--help`           | Print the help message.                                                                     |

The clean, delete, and rebuild options are used to manage the existing index. The clean option removes any documents
from the index that no longer exist in the database. The delete option deletes all records from the existing index. The
rebuild option wipes out the current index and rebuilds it from scratch.

The spellchecker option is used to rebuild the spellchecker for the index. This can be useful if you have made changes
to the index that affect the spellchecking functionality.

## Conclusion/QuickGuide:

The index-discovery script is a powerful tool for managing the Discovery Solr search index in DSpace. With this script,
you can add, update, or remove items, collections, or communities from the index, as well as clean, delete, or rebuild
the index from scratch. By using the various options available in the script, you can keep your Discovery Solr search
index up-to-date and ensure that your users are able to find the content they need in DSpace.
