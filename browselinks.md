# Browse Link Endpoint
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/discover/browselinks**   

Provide access to the browse system (SOLR based) similar to /browses/, but only returns the list of available browse indexes that are also configured as browse links.

Example: <http://dspace7.4science.it/dspace-spring-rest/#/dspace-spring-rest/api/discover/browselinks>

## Single browse index 
**/api/discover/browselinks/<:metadata-field>**

Provide detailed information about a specific browse index that is configured for the given field, if that field is configured
as a browse link.
```json
{
  "id": "author",
  "metadataBrowse": true,
  "dataType": "text",
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
    "dc.contributor.*",
    "dc.creator"
  ]
}
```

* id: the identifier for the browse index
* metadataBrowse: true if the browse index have two level, the 1st level shows the list of entries like author names, subjects, types, etc. the second level is the actual list of items linked to a specific entry
* dataType: the kind of data indexed. Can have the values "title" for item titles, "date" for date fields or "text" for other metadata
* sortOptions: the sort options available for this index
* order: the default order applied to the index
* metadata: the list of metadata used to build this index

Exposed links:
* items: link to get the actual list of items associated with the index
* entries (**only for metadata browse index**): link to get the list of entries in the metadata index

Error codes:

**404** if the browse index doesn't exist or is not configured as a browse link

## Browse entries

See [browses.md](browses.md) as embedded links point to this endpoint.

Error codes:

**404** if the browse index doesn't exist or if it is not configured as a browse link
**422** if both the page and startsWith parameters are present in the request
**500** 

1.  if a value or authority parameter **has been** specified and the browse index **is NOT** a metadata browse
2.	if a value or authority parameter has **NOT** been specified and the browse index **IS** a metadata browse
