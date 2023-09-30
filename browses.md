# Browse Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/discover/browses**   

Provide access to the browse system (SOLR based). It returns the list of available browse indexes.

Example: <https://{dspace.server.url}/api/discover/browses>

## Browse types

There are currently types of browse, each behaving differently:
* `valueList`:
  * The browse index has two levels
  * The 1st level shows the list of entries like author names, subjects, types, etc
  * The second level is the actual list of items linked to a specific entry
  * The `items` link is not used
  * The `entries` link is used to render the author names, subjects, types, etc
  * The `vocabulary` link is not used
* `flatBrowse`:
  * The browse index has one level: the full list of items
  * The `items` link is used to render this list of items
  * The `entries` link is not used
  * The `vocabulary` link is not used
* `hierarchicalBrowse`:
  * The browse index displays the vocabulary tree
  * The 1st level shows the tree
  * The second level is the actual list of items linked to a specific entry
  * The `items` link is not used
  * The `entries` link is not used
  * The `vocabulary` link is used to render the tree

## Single browse index 
**/api/discover/browses/<:index-name>**

Provide detailed information about a specific browse index and access to the list of items and entries in the index. The JSON response document is as follow
```json
{
  "id": "title",
  "browseType": "flatBrowse",
  "metadataBrowse": false,
  "dataType": "title",
  "sortOptions": [
    {
      "name": "title",
      "metadata": "dc.title"
    },
    {
      "name": "dateissued",
      "metadata": "dc.date.issued"
    },
    {
      "name": "dateaccessioned",
      "metadata": "dc.date.accessioned"
    }
  ],
  "order": "ASC",
  "type": "browse",
  "metadata": [
    "dc.date.issued"
  ]
} 
```

* id: the identifier for the browse index
* browseType:
  * `valueList` if the browse index has two levels, the 1st level shows the list of entries like author names, subjects, types, etc. the second level is the actual list of items linked to a specific entry
  * `flatBrowse` if the browse index has one level: the full list of items
* metadataBrowse: true if the browse index have two level, the 1st level shows the list of entries like author names, subjects, types, etc. the second level is the actual list of items linked to a specific entry
* dataType: the kind of data indexed. Can have the values "title" for item titles, "date" for date fields or "text" for other metadata
* sortOptions: the sort options available for this index
* order: the default order applied to the index
* metadata: the list of metadata used to build this index

Exposed links:
* items: link to get the actual list of items associated with the index
* entries (**only for metadata browse index**): link to get the list of entries in the metadata index

Error codes:

**404** if the browse index doesn't exist

## Hierarchical browse index 
**/api/discover/browses/<:index-name>**

Provide detailed information about a specific hierarchical browse index and access to the parameters required to access the list of items and entries. The JSON response document is as follows
```json
{
  "id": "keyword",
  "browseType": "hierarchicalBrowse",
  "facetType": "subject",
  "vocabulary": "srsc",
  "type": "browse",
  "metadata": [
    "dc.subject"
  ],
  "_links" : {
    "vocabulary" : {
      "href" : "/server/api/submission/vocabularies/srsc"
    }
  }
} 
```

* id: the identifier for the browse index
* browseType: `hierarchicalBrowse` if the browse index should display the vocabulary tree. The 1st level shows the tree. The second level is the actual list of items linked to a specific entry
* facetType: the discovery filter to use to filter the items
* vocabulary: the name of the vocabulary containing the tree
* metadata: the list of metadata used to build this index

Exposed links:
* vocabulary: link to the vocabulary containing the tree

Error codes:

**404** if the browse index doesn't exist

## Browse indexes configured for metadata linking
### Get a single index configured as browse links for the given metadata fields
**/api/discover/browses/search/byFields?fields=<:field>**

Example: <https://{dspace.server.url}/api/discover/browses/search/byFields?fields=dc.contributor.author&fields=dc.creator>

Provide detailed information about a specific browse index and access to the list of items and entries in the index. The JSON response document is as the previous section.

The supported parameters are:
* fields: A metadata field name, eg. dc.contributor.author. This parameter is repeatable.

Error codes:

**204** if none of the fields are configured for browse or browse links
**400** if the fields parameter is missing

## Browse entries
### Metadata browse 1st level
**/api/discover/browses/<:index-name>/entries**

Example: <https://{dspace.server.url}/api/discover/browses/author/entries>

It returns a collection of BrowseEntryResource the JSON document looks like
```json
{
  "_embedded": {
    "browseEntries": [
      {
        "authority": null,
        "value": "Arulmozhiyal, Ramaswamy",
        "type": "browseEntry",
        "valueLang": null,
        "count": 1,
        "_links": {
          "items": {
            "href": "https://demo.dspace.org/server/api/discover/browses/author/items?filterValue=Arulmozhiyal, Ramaswamy"
          }
        }
      },
      ...
      ]
  "_link": ...,
  "page": ...
  }
```

* authority: is the unique ID of the entry if the index is authority based. If not null it musts be used to retrieve the associated list of items. **Please note that HAL compliant client should follow the link "items" instead to build the link theirself**
* value: is the entry value to use, only if authority is null, to get the associated list of items. See also the above warning
* count: is the number of items associated with this entry

The supported parameters are:
* page, size & sort [see pagination](README.md#Pagination): the sort parameter must be specified as default,(asc|desc) as 1st level browse doesn't support sorting by option other than the natural sorting of the entries
* startsWith: value to use to filter the browse results so all results returned start with this prefix, so all plain-text browse entries start with this value
* scope: limit the browse to the items included in a specific DSpace container. It could be the UUID of a community or a collection. For example scope=9076bd16-e69a-48d6-9e41-0238cb40d863 

Error codes:

**404** if the browse index is not a metadata browse or the browse index doesn't exist at all
**422** if both the page and startsWith parameters are present in the request 

### Item browse or 2nd level metadata browse
**/api/discover/browses/<:index-name>/items**

Examples:

item browse: <https://{dspace.server.url}/api/discover/browses/dateissued/items>

2nd level metadata browse: <https://{dspace.server.url}/api/discover/browses/author/items?filterValue=Arulmozhiyal, Ramaswamy>

The supported parameters are:
* page, size & sort [see pagination](README.md#Pagination): the sort name must be one of the name specified in the sortOptions.name of the browse index or *default*, followed by a comma and the order direction. For example sort=default,asc or sort=dateissued,desc
* startsWith: value to use to filter the browse results so all results returned start with this prefix, so all items returned have name starting with this value
* scope: limit the browse to the items included in a specific DSpace container. It could be the UUID of a community or a collection. For example scope=9076bd16-e69a-48d6-9e41-0238cb40d863 

**On metadata browse** exactly one of the following parameter must be specified 
* value
* authority
**Please note that HAL compliant client should follow the link "items" in the browse entry resource instead to build the link theirself**

Error codes:

**404** if the browse index doesn't exist
**422** if both the page and startsWith parameters are present in the request
**500** 

1.  if a value or authority parameter **has been** specified and the browse index **is NOT** a metadata browse
2.	if a value or authority parameter has **NOT** been specified and the browse index **IS** a metadata browse
