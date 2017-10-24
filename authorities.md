# Authorities Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/authorities**   

Provide access to the configured authorities. It returns the list of existent authorities.

Example: to be provided

## Single Authority
**/api/integration/authorities/<:authority-name>**

Provide detailed information about a specific authority. The JSON response document is as follow
```json
{
  "name": "SolrAuthorAuthority",
  "hierarchical": false,
  "scrollable": false
}
```

Exposed links:
* entries: the list of values managed by the authority

## Linked entities
### authority entries
**/api/integration/authorities/<:authority-name>/entries **

It returns the entries managed by the authority eventually filtered, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* metadata: the metadata that use the authority
* query: the terms, keywords or prefix to search
* parent: the key of the parent authority when searching in a hierarchical authority 
* collection: the uuid of the collection where the item belong to

It returns the entries in the authority matching the query

sample for an authority 
```json
{
	authorityEntries: [
		{
		  "id": "rp00001",
		  "display": "Surname, Lastname",
		  "otherInformation": 
		  	{
		    	ORCID: "0000-0000-0000-0000"
		      	affiliation: "University of Sample",
		      	biography: "...",
		      	"count": 19
		    }
	    },
	    {
		  "id": "rp00002",
		  "display": "Other, Researcher",
		  "otherInformation": 
		  	{
		    	"ORCID": "0000-0000-0000-0000"
		      	"affiliation": "My University",
		      	"biography": "...",
		      	"count": 3
		    }
	    },
}
```

sample for a hierarchical authority  (srsc)
```json
{
	authorityEntries: [
		{
		  "id": "SCB110"
		  "display": "History of religion",
		  "otherInformation": 
		  	{
		    	"note": "Religionshistoria",
		    	"count": 19
		    }
	    },
	    {
		  "id": "VR110103"
		  "display": "Other, Researcher",
		  "otherInformation": 
		  	{
		    	"note": "Kyrkovetenskap",
		    	"count": 3
		    }
	    },
	    ...
	]
}
```
