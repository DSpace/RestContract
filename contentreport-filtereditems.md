# Metadata query (aka Filtered Items) report
[Back to the list of all defined endpoints](endpoints.md)

This endpoint provides a custom query API to select items from existing collections,
according to given Boolean and metadata filters.

NOTE: This is currently a beta feature.


**GET /api/contentreport/filtereditems**

The report parameters are described [below](#report-parameterization).

Additionally, a `pageNumber` parameter is available to retrieve results starting at a given page
(according to `pageLimit`, the maximum number of items per page). Page numbering starts at 0.

All parameters except `pageNumber` and `pageLimit` are repeatable. Multiple values can be expressed either
by repeating the corresponding parameter, e.g.:
```
?filters=is_discoverable&filters=has_multiple_originals&filters=has_pdf_original
```

of by using a comma-separated value, e.g.:

```
?filters=is_discoverable,has_multiple_originals,has_pdf_original
```

except the `queryPredicates` parameter, which supports only parameter repetition for multiple values
to avoid any ambiguities in case a predicate values contains commas.

Please see [below](#report-parameterization) for parameterization details.

## Report contents

An example JSON response document to `/api/contentreport/filtereditems` (metadata removed for brevity):
```json
{
    "id": "filtereditems",
    "items": [
        {
            "id": "07e388ff-f22b-4d4f-8275-acab5c3edacc",
            "uuid": "07e388ff-f22b-4d4f-8275-acab5c3edacc",
            "name": "Enhancing the lubricity of an environmentally friendly Swedish diesel fuel MK1",
            "handle": "20.500.11794/42",
            "metadata": {
                "dc.contributor.author": [
                    {
                        "value": "Smith, John",
                        "language": null,
                        "authority": "6eee383a-f126-4705-9ffb-b4aa4832070e",
                        "confidence": 600,
                        "place": 0
                    }
                ],
                "dc.publisher": [
                    {
                        "value": "Elsevier",
                        "language": "fr_CA",
                        "authority": null,
                        "confidence": -1,
                        "place": 0
                    }
                ],
            },
            "inArchive": true,
            "discoverable": true,
            "withdrawn": false,
            "lastModified": "2015-11-23T17:30:21.463+00:00",
            "entityType": "Publication",
            "owningCollection": {
                "id": "d98a828c-45c2-43d9-9861-6b9800bf14f5",
                "uuid": "d98a828c-45c2-43d9-9861-6b9800bf14f5",
                "name": "Articles publiés dans des revues avec comité de lecture",
                "handle": "100/1",
                "metadata": {
                    "dc.identifier.uri": [
                        {
                            "value": "http://localhost:4000/handle/100/1",
                            "language": null,
                            "authority": null,
                            "confidence": -1,
                            "place": 0
                        }
                    ],
                    "dspace.entity.type": [
                        {
                            "value": "Publication",
                            "language": null,
                            "authority": null,
                            "confidence": -1,
                            "place": 0
                        }
                    ]
                },
                "type": "collection"
            },
            "type": "item"
        },
        {
            ...
        }
    ],
    "itemCount": 40,
    "type": "filtereditemsreport",
    "_links": {
        "self": {
            "href": "http://localhost:8080/dspace-server/api/contentreport/filtereditems"
        }
    }
}
```

## Report parameterization

The parameters are specified as follows:

* `collections`: The collection UUIDs where to search items. If none are provided, the whole repository is searched.
* `presetQuery`: This parameter is not used on the REST API side. It defines a predefined set of query predicates
  defined in the Angular layer.
* `queryPredicates`: Predicates used to filter matching items. They can be predefined (see `presetQuery` above)
  or defined specifically by the user. As mentioned above, they are the only parameter that cannot be repeated
  using comma-separated values.
* `pageLimit`: Maximum number of items per page.
* `filters`: Supplementary filters, these are the same as those available in the Filtered Collections report.
  Please see [/api/contentreport/filteredcollections](contentreport-filteredcollections.md#available-filters) for details.
* `additionalFields`: Fields to add to the basic report for each item included in the report.

The _basic report_ mentioned above includes, for each item:

* Sequential number (order of appearance in the report)
* UUID
* Parent collection
* Handle
* Title

Possible response status:

* 200 OK - The specific report data was found, and the data has been properly returned.
* 403 Forbidden - In case of unauthorized user session.
