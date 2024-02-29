# Item Filter Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint contains the various logical item filters.

## Main Endpoint
**/api/config/itemfilters**

List all the available item filters in the system.

```json
{
  "_embedded" : {
    "itemfilters" : [ {
      "id" : "a-common-or_statement",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/a-common-or_statement"
        }
      }
    }, {
      "id" : "always_true_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/always_true_filter"
        }
      }
    }, {
      "id" : "dc-identifier-uri-contains-doi_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/dc-identifier-uri-contains-doi_condition"
        }
      }
    }, {
      "id" : "demo_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/demo_filter"
        }
      }
    }, {
      "id" : "doi-filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/doi-filter"
        }
      }
    }, {
      "id" : "driver-document-type_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/driver-document-type_condition"
        }
      }
    }, {
      "id" : "example-doi_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/example-doi_filter"
        }
      }
    }, {
      "id" : "has-at-least-one-bitstream_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/has-at-least-one-bitstream_condition"
        }
      }
    }, {
      "id" : "has-bitstream_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/has-bitstream_filter"
        }
      }
    }, {
      "id" : "has-one-bitstream_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/has-one-bitstream_condition"
        }
      }
    }, {
      "id" : "in-outfit-collection_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/in-outfit-collection_condition"
        }
      }
    }, {
      "id" : "is-archived_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/is-archived_condition"
        }
      }
    }, {
      "id" : "is-withdrawn_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/is-withdrawn_condition"
        }
      }
    }, {
      "id" : "item-is-public_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/item-is-public_condition"
        }
      }
    }, {
      "id" : "openaire_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/openaire_filter"
        }
      }
    }, {
      "id" : "simple-demo_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/simple-demo_filter"
        }
      }
    }, {
      "id" : "title-contains-demo_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/title-contains-demo_condition"
        }
      }
    }, {
      "id" : "title-starts-with-pattern_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/title-starts-with-pattern_condition"
        }
      }
    }, {
      "id" : "type-equals-dataset_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/type-equals-dataset_condition"
        }
      }
    }, {
      "id" : "type-equals-journal-article_condition",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/type-equals-journal-article_condition"
        }
      }
    }, {
      "id" : "type_filter",
      "type" : "itemfilter",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/config/itemfilters/type_filter"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/config/itemfilters?size=30"
    }
  },
  "page" : {
    "size" : 30,
    "totalElements" : 21,
    "totalPages" : 1,
    "number" : 0
  }
}
```

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators

## Single Item Filter
**/api/config/itemfilters/<:id>**

Status codes:
* 200 OK - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged in with sufficient permissions. Only administrators
* 404 Not found - if the itemfilter doesn't exist

