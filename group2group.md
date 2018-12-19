# Group 2 Group mapping Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/group2group**

Filter using parameters:
* parent : UUID of the parent group (mutually exclusive with child)
* child : UUID of the child group (mutually exclusive with parent)
* all : retrieve idirect parent or child relations as well

Samples:
* GET /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42
* GET /rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42&all=true
* GET /rest/api/eperson/group2group?child=ddca960c-2033-4cf0-9430-5918d94751ef
* GET /rest/api/eperson/group2group?child=ddca960c-2033-4cf0-9430-5918d94751ef&all=true

```json
"_embedded": {
  "groups": [
    {
      "id": "ddca960c-2033-4cf0-9430-5918d94751ef",
      "uuid": "ddca960c-2033-4cf0-9430-5918d94751ef",
      "name": "COLLECTION_1_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_1_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/ddca960c-2033-4cf0-9430-5918d94751ef/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "1cbf084f-a3d1-423c-949c-b5ae96859d8b",
      "uuid": "1cbf084f-a3d1-423c-949c-b5ae96859d8b",
      "name": "COLLECTION_16_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_16_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/1cbf084f-a3d1-423c-949c-b5ae96859d8b/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "0de88b0a-8b57-49f3-ae5f-b5752ff79760",
      "uuid": "0de88b0a-8b57-49f3-ae5f-b5752ff79760",
      "name": "COLLECTION_14_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_14_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/0de88b0a-8b57-49f3-ae5f-b5752ff79760/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/0de88b0a-8b57-49f3-ae5f-b5752ff79760"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/0de88b0a-8b57-49f3-ae5f-b5752ff79760/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "7b0cac8f-edf9-493a-9a94-29c2f2cd8885",
      "uuid": "7b0cac8f-edf9-493a-9a94-29c2f2cd8885",
      "name": "COLLECTION_17_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_17_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/7b0cac8f-edf9-493a-9a94-29c2f2cd8885/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/7b0cac8f-edf9-493a-9a94-29c2f2cd8885"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/7b0cac8f-edf9-493a-9a94-29c2f2cd8885/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "34f2a34d-bbf7-4446-bede-f2c0e81badd9",
      "uuid": "34f2a34d-bbf7-4446-bede-f2c0e81badd9",
      "name": "COLLECTION_9_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_9_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/34f2a34d-bbf7-4446-bede-f2c0e81badd9/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/34f2a34d-bbf7-4446-bede-f2c0e81badd9"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/34f2a34d-bbf7-4446-bede-f2c0e81badd9/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "05e3dbb8-332b-4487-a3f9-d78431b6cc02",
      "uuid": "05e3dbb8-332b-4487-a3f9-d78431b6cc02",
      "name": "COLLECTION_10_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_10_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/05e3dbb8-332b-4487-a3f9-d78431b6cc02/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/05e3dbb8-332b-4487-a3f9-d78431b6cc02"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/05e3dbb8-332b-4487-a3f9-d78431b6cc02/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "41a3825e-60d1-4873-846e-4e87b44406d2",
      "uuid": "41a3825e-60d1-4873-846e-4e87b44406d2",
      "name": "COLLECTION_6_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_6_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/41a3825e-60d1-4873-846e-4e87b44406d2/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/41a3825e-60d1-4873-846e-4e87b44406d2"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/41a3825e-60d1-4873-846e-4e87b44406d2/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "c8d86def-f3c2-4439-9761-3e8fe353684d",
      "uuid": "c8d86def-f3c2-4439-9761-3e8fe353684d",
      "name": "COLLECTION_8_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_8_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/c8d86def-f3c2-4439-9761-3e8fe353684d/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/c8d86def-f3c2-4439-9761-3e8fe353684d"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/c8d86def-f3c2-4439-9761-3e8fe353684d/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "f22c425c-e1a7-4d5e-af01-8e386e370047",
      "uuid": "f22c425c-e1a7-4d5e-af01-8e386e370047",
      "name": "COLLECTION_7_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_7_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/f22c425c-e1a7-4d5e-af01-8e386e370047/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/f22c425c-e1a7-4d5e-af01-8e386e370047"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/f22c425c-e1a7-4d5e-af01-8e386e370047/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    },
    {
      "id": "a81d09d6-813b-4404-bfb5-8f3423c711db",
      "uuid": "a81d09d6-813b-4404-bfb5-8f3423c711db",
      "name": "COLLECTION_13_WORKFLOW_ROLE_reviewer",
      "handle": null,
      "metadata": [
        {
          "key": "dc.title",
          "value": "COLLECTION_13_WORKFLOW_ROLE_reviewer",
          "language": null
        }
      ],
      "permanent": false,
      "type": "group",
      "_links": {
        "groups": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/a81d09d6-813b-4404-bfb5-8f3423c711db/groups"
        },
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/a81d09d6-813b-4404-bfb5-8f3423c711db"
        }
      },
      "_embedded": {
        "groups": {
          "_embedded": {
            "groups": []
          },
          "_links": {
            "self": {
              "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/a81d09d6-813b-4404-bfb5-8f3423c711db/groups"
            }
          },
          "page": {
            "number": 0,
            "size": 0,
            "totalPages": 1,
            "totalElements": 0
          }
        }
      }
    }
  ]
},
"_links": {
  "self": {
    "href": "https://dspace7-internal.atmire.com/rest/api/eperson/group2group?parent=3b1de75d-5bf9-4ca3-bf4d-42bc8abd0d42/groups"
  }
},
"page": {
  "number": 0,
  "size": 10,
  "totalPages": 1,
  "totalElements": 10
}
}
```
