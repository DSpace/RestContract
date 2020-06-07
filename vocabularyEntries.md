# Vocabulary Entries Endpoints
[Back to the list of all defined endpoints](endpoints.md)

All the endpoints here described are restricted to authenticated users.

## Main Endpoint
**/api/integration/vocabularyEntries**   

Not allowed. Only subset of vocabulary entries can be retrieved using specific filters see below

## Single Authority
**/api/integration/vocabularyEntries/<vocabulary-name><:entry-id>**

Provide detailed information about a specific vocabulary entry. The JSON response document is as follow
```json
{
  "id": "srsc",
  "name": "srsc",
  "scrollable": false,
  "hierarchical": true,
  "type": "authority"
}
```

Exposed links:
* vocabulary: the vocabulary that the entry belong to
* parent: for hierarchical vocabulary, the entry parent of the current entry, if any
* childrens: for hierarchical vocabulary, the list of children entries, if any

Return codes:
* 200 OK - if the operation succeed
* 404 Not Found - if the vocabulary entry is missing or the resource ID is malformed
* 401 Unauthorized - if you are not authenticated

## Search methods
### top
**/api/integration/vocabularyEntries/search/top?vocabulary=<:vocabulary-id>**

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* vocabulary: the id of the vocabulary to query. It must be an hierarchical vocabulary

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the vocabulary parameter is missing or invalid
* 401 Unauthorized - if you are not authenticated
* 422 Unprocessable Entity - if the specified vocabulary exists but it is not an hierarchical vocabulary

Provide detailed information about all top entries of a specific hierarchical vocabulary. The JSON response document is as follow
```json, this one represent all top entries of "srsc" vocabulary
{
      "id" : "srsc:SCB11",
      "value" : "HUMANITIES and RELIGION",
      "selectable" : true,
      "otherInformation" : {
          "id": "SCB11",
          "note" : "HUMANIORA och RELIGIONSVETENSKAP",
      },
      "type" : "vocabularyEntry",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/integration/vocabularyEntries/srsc:SCB11"
        },
        "vocabulary" : {
          "href" : "http://localhost/api/integration/vocabularies/srsc"
        },
        "childrens" : {
          "href" : "http://localhost/api/integration/vocabularyEntries/srsc:SCB11/childrens"
        },
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
