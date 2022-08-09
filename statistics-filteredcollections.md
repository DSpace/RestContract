# Displaying filtered collections statistics
[Back to the list of all defined endpoints](endpoints.md)

## Statistics for the whole repository
**GET /api/statistics/filteredcollections**

**POST /api/statistics/filteredcollections**

This endpoint provides aggregated statistics about the number of items per collection according to selected filters.

For each collection, the basic report consists of:
* name (label) and handle of the collection
* name (label) and handle of the parent community
* total number of items
* number of items matching all selected filters

In addition, a `summary` element provides the total number of items and the total number of items matching all filters
for the whole repository.

An example JSON response document to `/api/statistics/filteredcollections`:
```json
{
    "id": null,
    "collections": [
        {
            "label": "Collection 1",
            "handle": "100/1",
            "values": {
                "is_discoverable": 23,
                "has_multiple_originals": 3,
                "has_pdf_original": 14
            },
            "community_label": "Community 1",
            "community_handle": "20.500.11794/1",
            "nb_total_items": 23,
            "all_filters_value": 3
        },
        {
            "label": "Collection 2",
            "handle": "100/2",
            "values": {
                "is_discoverable": 1,
                "has_multiple_originals": 0,
                "has_pdf_original": 0
            },
            "community_label": "Community 1",
            "community_handle": "20.500.11794/1",
            "nb_total_items": 1,
            "all_filters_value": 0
        },
        {
            "label": "Collection 3",
            "handle": "100/3",
            "values": {
                "is_discoverable": 1,
                "has_multiple_originals": 0,
                "has_pdf_original": 1
            },
            "community_label": "Community 1",
            "community_handle": "20.500.11794/1",
            "nb_total_items": 1,
            "all_filters_value": 0
        }
    ],
    "summary": {
        "label": null,
        "handle": null,
        "values": {
            "is_discoverable": 25,
            "has_multiple_originals": 3,
            "has_pdf_original": 15
        },
        "community_label": null,
        "community_handle": null,
        "nb_total_items": 25,
        "all_filters_value": 3
    },
    "type": "filtered-collections",
    "_links": {
        "self": {
            "href": "http://localhost:8080/dspace-server/api/statistics/filtered-collections"
        }
    }
}
```

The request can be parameterized with a series of filters to add to the basic report.

In GET mode, it consists of a `filters` query parameter whose value is a comma-separated list of filters
like the following:
```
?filters=is_discoverable,has_multiple_originals,has_pdf_original
```

In POST mode, it is defined as a JSON document like this:
```json
{
    "filters": {
        "is_discoverable": true,
        "has_multiple_originals": true,
        "has_pdf_original": true
    }
}
```

The available filters are as follows:

* Item Property Filters
    * `is_item`: Is Item - always true
    * `is_withdrawn`: Withdrawn Items
    * `is_not_withdrawn`: Available Items - Not Withdrawn
    * `is_discoverable`: Discoverable Items - Not Private
    * `is_not_discoverable`: Not Discoverable - Private Item
* Basic Bitstream Filters
    * `has_multiple_originals`: Item has Multiple Original Bitstreams
    * `has_no_originals`: Item has No Original Bitstreams
    * `has_one_original`: Item has One Original Bitstream
* Bitstream Filters by MIME Type
    * `has_doc_original`: Item has a Doc Original Bitstream (PDF, Office, Text, HTML, XML, etc)
    * `has_image_original`: Item has an Image Original Bitstream
    * `has_unsupp_type`: Has Other Bitstream Types (not Doc or Image)
    * `has_mixed_original`: Item has multiple types of Original Bitstreams (Doc, Image, Other)
    * `has_pdf_original`: Item has a PDF Original Bitstream
    * `has_jpg_original`: Item has JPG Original Bitstream
    * `has_small_pdf`: Has unusually small PDF
    * `has_large_pdf`: Has unusually large PDF
    * `has_doc_without_text`: Has document bitstream without TEXT item
* Supported MIME Type Filters
    * `has_only_supp_image_type`: Item Image Bitstreams are Supported
    * `has_unsupp_image_type`: Item has Image Bitstream that is Unsupported
    * `has_only_supp_doc_type`: Item Document Bitstreams are Supported
    * `has_unsupp_doc_type`: Item has Document Bitstream that is Unsupported
* Bitstream Bundle Filters
    * `has_unsupported_bundle`: Has bitstream in an unsupported bundle
    * `has_small_thumbnail`: Has unusually small thumbnail
    * `has_original_without_thumbnail`: Has original bitstream without thumbnail
    * `has_invalid_thumbnail_name`: Has invalid thumbnail name (assumes one thumbnail for each original)
    * `has_non_generated_thumb`: Has non-generated thumbnail
    * `no_license`: Doesn't have a license
    * `has_license_documentation`: Has documentation in the license bundle
* Permission Filters
    * `has_restricted_original`: Item has Restricted Original Bitstream
    * `has_restricted_thumbnail`: Item has Restricted Thumbnail
    * `has_restricted_metadata`: Item has Restricted Metadata

Possible response status

* 200 OK - The specific statistics data was found, and the data has been properly returned.
* 403 Forbidden - if a valid CSRF token is missing when issuing a POST request.
