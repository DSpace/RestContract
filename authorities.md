# Authorities Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/integration/authorities**   

Provide access to the configured authorities. It returns the list of existent authorities.

Example:
```json
{
  "_embedded": {
    "authorities": [
      {
        "id": "srsc",
        "name": "srsc",
        "scrollable": false,
        "hierarchical": true,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc"
          }
        }
      },
      {
        "id": "common_types",
        "name": "common_types",
        "scrollable": false,
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types"
          }
        }
      },
      {
        "id": "common_iso_languages",
        "name": "common_iso_languages",
        "scrollable": false,
        "hierarchical": false,
        "type": "authority",
        "_links": {
          "entryValues": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages/entryValues"
          },
          "entries": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages/entries"
          },
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_iso_languages"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 3,
    "totalPages": 1,
    "number": 0
  }
}
```

## Single Authority
**/api/integration/authorities/<:authority-name>**

Provide detailed information about a specific authority. The JSON response document is as follow
```json
{
  "id": "srsc",
  "name": "srsc",
  "scrollable": false,
  "hierarchical": true,
  "type": "authority"
}
```
Attributes
* id: the id of the authority, it is the same than the name
* name: see id
* scrollable: if true mean that it is possible to scroll all the entries in the authority without providing a query parameter, see (authority entries)[authorities.md#authority-entries]
* hierarchical: if true means that the authority expose a tree structure where come entries are parent of others
* preloadLevel: for hierarchical authorities express the preference to preload the tree at a specific level of depth (0 only the top nodes are shown, 1 also their childrens are preloaded and so on)

Exposed links:
* entries: the list of values managed by the authority
* entryValues: the endpoint to retrieve a single value

## Top Authority
**/api/integration/authorities/<:authority-name>/entries/search/top**
The supported parameters are:
* page, size [see pagination](README.md#Pagination)

Provide detailed information about all top choices of specific authority. The JSON response document is as follow
```json, this one rappresent all top choices of "srsc" authority
{
      "id" : "SCB11",
      "display" : "HUMANITIES and RELIGION",
      "value" : "HUMANITIES and RELIGION",
      "selectable" : true,
      "otherInformation" : {
        "note" : "HUMANIORA och RELIGIONSVETENSKAP",
        "children" : "SCB110::SCB111::SCB112::SCB113::SCB119"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB11"
        }
      }
    }, {
      "id" : "SCB12",
      "display" : "LAW/JURISPRUDENCE",
      "value" : "LAW/JURISPRUDENCE",
      "selectable" : true,
      "otherInformation" : {
        "note" : "RÄTTSVETENSKAP/JURIDIK",
        "children" : "SCB1201::SCB1202::SCB1203::SCB1204::SCB1205::SCB1209"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB12"
        }
      }
    }, {
      "id" : "SCB13",
      "display" : "SOCIAL SCIENCES",
      "value" : "SOCIAL SCIENCES",
      "selectable" : true,
      "otherInformation" : {
        "note" : "SAMHÄLLSVETENSKAP",
        "children" : "SCB131::SCB132::SCB133::SCB139"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB13"
        }
      }
    }, {
      "id" : "SCB14",
      "display" : "MATHEMATICS",
      "value" : "MATHEMATICS",
      "selectable" : true,
      "otherInformation" : {
        "note" : "MATEMATIK",
        "children" : "SCB1401::SCB1402::SCB1409"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB14"
        }
      }
    }, {
      "id" : "SCB15",
      "display" : "NATURAL SCIENCES",
      "value" : "NATURAL SCIENCES",
      "selectable" : true,
      "otherInformation" : {
        "note" : "NATURVETENSKAP",
        "children" : "SCB150::SCB151::SCB152::SCB153"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB15"
        }
      }
    }, {
      "id" : "SCB16",
      "display" : "TECHNOLOGY",
      "value" : "TECHNOLOGY",
      "selectable" : true,
      "otherInformation" : {
        "note" : "TEKNIKVETENSKAP",
        "children" : "SCB160::SCB161::SCB162::SCB163::SCB164::SCB165::SCB166::SCB167::SCB168::SCB169"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB16"
        }
      }
    }, {
      "id" : "SCB17",
      "display" : "FORESTRY, AGRICULTURAL SCIENCES and LANDSCAPE PLANNING",
      "value" : "FORESTRY, AGRICULTURAL SCIENCES and LANDSCAPE PLANNING",
      "selectable" : true,
      "otherInformation" : {
        "note" : "SKOGS- och JORDBRUKSVETENSKAP samt LANDSKAPSPLANERING",
        "children" : "SCB171::SCB172::SCB173::SCB174::SCB175::SCB176::SCB177"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB17"
        }
      }
    }, {
      "id" : "SCB18",
      "display" : "MEDICINE",
      "value" : "MEDICINE",
      "selectable" : true,
      "otherInformation" : {
        "note" : "MEDICIN",
        "children" : "SCB180::SCB181::SCB182::SCB183::SCB184::SCB185::SCB186::SCB187"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB18"
        }
      }
    }, {
      "id" : "SCB19",
      "display" : "ODONTOLOGY",
      "value" : "ODONTOLOGY",
      "selectable" : true,
      "otherInformation" : {
        "note" : "ODONTOLOGI",
        "children" : "SCB1901::SCB1902::SCB1903::SCB1904::SCB1905::SCB1906::SCB1907::SCB1908::SCB1911::SCB1912::SCB1913::SCB1914::SCB1915::SCB1916::SCB1917::SCB1918::SCB1921::SCB1922::SCB1929"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB19"
        }
      }
    }, {
      "id" : "SCB21",
      "display" : "PHARMACY",
      "value" : "PHARMACY",
      "selectable" : true,
      "otherInformation" : {
        "note" : "FARMACI",
        "children" : "SCB2101::SCB2102::SCB2103::SCB2104::SCB2105::SCB2106::SCB2107::SCB2108::SCB2111::SCB2112::SCB2119"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB21"
        }
      }
    }, {
      "id" : "SCB22",
      "display" : "VETERINARY MEDICINE",
      "value" : "VETERINARY MEDICINE",
      "selectable" : true,
      "otherInformation" : {
        "note" : "VETERINÄRMEDICIN",
        "children" : "SCB2201::SCB2202::SCB2203::SCB2204::SCB2205::SCB2206::SCB2207::SCB2208::SCB2211::SCB2212::SCB2213::SCB2214::SCB2215::SCB2216::SCB2217::SCB2219"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB22"
        }
      }
    }, {
      "id" : "SCB23",
      "display" : "INTERDISCIPLINARY RESEARCH AREAS",
      "value" : "INTERDISCIPLINARY RESEARCH AREAS",
      "selectable" : true,
      "otherInformation" : {
        "note" : "TVÄRVETENSKAPLIGA FORSKNINGSOMRÅDEN",
        "children" : "SCB231::SCB232::SCB233::SCB234::SCB235::SCB236::SCB237::SCB238::SCB239::SCB241::SCB242::SCB243"
      },
      "type" : "authorityEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB23"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/integration/authorities/srsc/entries/search/top"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 12,
    "totalPages" : 1,
    "number" : 0
  }

## byParent Authority
**/api/integration/authorities/<:authority-name>/entries/search/byParent?String=<:id>**
The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* id: mandatory, rappresent the prefered value of authority choice

It returns all values from the authority that match the preferred value
Esample for srsc authority /server//api/integration/authorities/srsc/entries/search/byParent?id=SCB14
{
      "id" : "SCB1401",
      "display" : "Algebra, geometry and mathematical analysis",
      "value" : "Algebra, geometry and mathematical analysis",
      "selectable" : true,
      "otherInformation" : {
        "parent" : "SCB14",
        "note" : "Algebra, geometri och analys",
        "children" : "VR140102::VR140103::VR140104::VR140105"
      },
      "type" : "authorityEntry",
      "_links" : {
        "parent" : {
          "href" : "http://localhost/api/integration/authorities/srsc/entryValues/SCB14"
        },
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB1401"
        }
      }
    }, {
      "id" : "SCB1402",
      "display" : "Applied mathematics",
      "value" : "Applied mathematics",
      "selectable" : true,
      "otherInformation" : {
        "parent" : "SCB14",
        "note" : "Tillämpad matematik",
        "children" : "VR140202::VR140203::VR140204::VR140205"
      },
      "type" : "authorityEntry",
      "_links" : {
        "parent" : {
          "href" : "http://localhost/api/integration/authorities/srsc/entryValues/SCB14"
        },
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB1402"
        }
      }
    }, {
      "id" : "SCB1409",
      "display" : "Other mathematics",
      "value" : "Other mathematics",
      "selectable" : true,
      "otherInformation" : {
        "parent" : "SCB14",
        "note" : "Övrig matematik"
      },
      "type" : "authorityEntry",
      "_links" : {
        "parent" : {
          "href" : "http://localhost/api/integration/authorities/srsc/entryValues/SCB14"
        },
        "self" : {
          "href" : "http://localhost/api/integration/authorityEntries/srsc/entryValues/SCB1409"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/integration/authorities/srsc/entries/search/byParent"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 3,
    "totalPages" : 1,
    "number" : 0
  }



## Linked entities
### authority entries
**/api/integration/authorities/<:authority-name>/entries**

It returns the filtered entries managed by the authority, see below 

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* metadata: the metadata field for which the authority is used: mandatory
* query: the terms, keywords or prefix to search: mandatory
* parent: the key of the parent authority when searching in a hierarchical authority 
* collection: the uuid of the collection where the item belong to

It returns the entries in the authority matching the query

TODO: the example below returns too many responses, and doesn't contain Book as the top result

sample for an authority /server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&size=2 
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "Dataset",
        "display": "Dataset",
        "value": "Dataset",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Dataset"
          }
        }
      },
      {
        "id": "Image, 3-D",
        "display": "Image, 3-D",
        "value": "Image, 3-D",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Image%2C%203-D"
          }
        }
      },
      {
        "id": "Book",
        "display": "Book",
        "value": "Book",
        "otherInformation": {},
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues/Book"
          }
        }
      }
    ]
  },
  "_links": {
    "first": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=0&size=2"
    },
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&size=2"
    },
    "next": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=1&size=2"
    },
    "last": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entries?metadata=dc.type&query=Book&page=10&size=2"
    }
  },
  "page": {
    "size": 2,
    "totalElements": 22,
    "totalPages": 11,
    "number": 0
  }
}
```

sample for a hierarchical authority  (srsc): /server/api/integration/authorities/srsc/entries?metadata=dc.subject&query=Research&size=2
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "VR131402",
        "display": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "value": "Research Subject Categories::SOCIAL SCIENCES::Social sciences::Social work::Family research",
        "otherInformation": {
          "parent": "SCB1314",
          "note": "Familjeforskning"
        },
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/VR131402"
          },
          "parent": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/SCB1314"
          }
        }
      },
      {
        "id": "ResearchSubjectCategories",
        "display": "Research Subject Categories",
        "value": "Research Subject Categories",
        "otherInformation": {
          "note": "Ämneskategorier för vetenskapliga publikationer"
        },
        "type": "authority",
        "_links": {
          "self": {
            "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entryValues/ResearchSubjectCategories"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/srsc/entries?metadata=dc.subject&query=Research&size=2"
    }
  },
  "page": {
    "size": 2,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

### authority entry values
**/api/integration/authorities/<:authority-name>/entryValues/<:entry-id>**

It returns the data from one entry in an authority

sample for an authority /server/api/integration/authorities/common_types/entryValues/Book 
```json
{
  "_embedded": {
    "authorityEntries": [
      {
        "id": "Book",
        "display": "Book",
        "value": "Book",
        "otherInformation": {},
        "type": "authority"
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://dspace7.4science.cloud/server/api/integration/authorities/common_types/entryValues"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 1,
    "totalPages": 1,
    "number": 0
  }
}
```
