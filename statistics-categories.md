# Displaying statistics categories
[Back to the list of all defined endpoints](endpoints.md)

## Statistics categories
**GET /api/statistics/categories**

As we don't have a use case to iterate over all the existing categories, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#error-codes).

## Single statistics category
**GET /api/statistics/categories/<:id>**

This endpoint provides a specific statistics category.

An example JSON response document to `/api/statistics/categories/site-mainReports`:

```json
{
    "id": "site-mainReports",
    "type": "category",
    "uniqueType": "statistics.category",
    "category-type": "mainReports",
    "_links": {
        "self": {
            "href": "https://{dspace.url}/server/api/statistics/categories/site-mainReports"
        }
    }
}
```

The `id` is the identifier of the category as defined in the statistics configuration, and the `category-type` is the type used to group and label the category (typically for internationalization). The two values often match, but they are not required to: for example, the `item-mainReports` category has `category-type` `mainReports`.

The following categories are defined in the default configuration:

| Category id | Category type | Applies to |
|---|---|---|
| `site-mainReports` | `mainReports` | Site |
| `site-downloadReports` | `downloadReports` | Site |
| `community-mainReports` | `mainReports` | Community |
| `community-itemReports` | `itemReports` | Community |
| `community-downloadReports` | `downloadReports` | Community |
| `collection-mainReports` | `mainReports` | Collection |
| `collection-itemReports` | `itemReports` | Collection |
| `collection-downloadReports` | `downloadReports` | Collection |
| `publicationCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Publication` |
| `projectCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Project` |
| `fundingCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Funding` |
| `orgUnitCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `OrgUnit` |
| `productCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Product` |
| `patentCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Patent` |
| `eventCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Event` |
| `personCollection-itemReports` | `itemReports` | Collection with `dspace.entity.type` = `Person` |
| `item-mainReports` | `mainReports` | Item |
| `bitstream-mainReports` | `mainReports` | Bitstream |
| `person-mainReports`, `person-publicationsReports`, `person-projectsReports` | `mainReports`, `publicationsReports`, `projectsReports` | Item with `dspace.entity.type` = `Person` |
| `publication-mainReports` | `mainReports` | Item with `dspace.entity.type` = `Publication` |
| `project-mainReports`, `project-publicationsReports` | `mainReports`, `publicationsReports` | Item with `dspace.entity.type` = `Project` |
| `affiliation-mainReports`, `affiliation-publicationsReports`, `affiliation-rpPublicationsReports`, `affiliation-projectsReports`, `affiliation-rpProjectsReports` | `mainReports`, `publicationsReports`, `rppublicationsReports`, `projectsReports`, `rpprojectsReports` | Item with `dspace.entity.type` = `OrgUnit` |

Categories whose `id` contains an entity type (for example `person-mainReports`) are only returned for objects of that entity type. Additional categories can be defined in the statistics configuration; the table above only lists the defaults.

Possible response status:

* 200 OK - The specific statistics category has been found, and the category has been properly returned.
* 404 Not Found - The specified ID was not found.

## Search statistics categories for a DSpaceObject
**GET /api/statistics/categories/search/object**

This endpoint provides a paginated list of the statistics categories configured for a DSpaceObject. The DSpaceObject is given through the following parameters:

* `uri` - The object to retrieve statistics categories for. The full URI of the REST resource must be specified, i.e. `https://{dspace.url}/server/api/core/items/{uuid}`. This parameter is required.

The usual parameters for paginated lists are supported as well:

* `page` - The page number
* `size` - The number of categories in a page

Note that, unlike [`/api/statistics/usagereports/search/object`](statistics-reports.md), this endpoint does **not** support the `startDate` and `endDate` parameters. Those parameters only exist on the usage reports search endpoint, where the `category` parameter can be used to restrict the returned reports to a single category (see [statistics-reports.md](statistics-reports.md)).

An example JSON response document to `/api/statistics/categories/search/object?page=0&size=2&uri=https://{dspace.url}/server/api/core/site/6d65c6a2-3fe7-44dd-bacb-79271257c35d`:

```json
{
    "page": {
        "number": 0,
        "size": 2,
        "totalPages": 1,
        "totalElements": 2
    },
    "_embedded": {
        "categories": [
            {
                "id": "site-mainReports",
                "type": "category",
                "uniqueType": "statistics.category",
                "category-type": "mainReports",
                "_links": {
                    "self": {
                        "href": "https://{dspace.url}/server/api/statistics/categories/site-mainReports"
                    }
                }
            },
            {
                "id": "site-downloadReports",
                "type": "category",
                "uniqueType": "statistics.category",
                "category-type": "downloadReports",
                "_links": {
                    "self": {
                        "href": "https://{dspace.url}/server/api/statistics/categories/site-downloadReports"
                    }
                }
            }
        ]
    }
}
```

An example JSON response document to `/api/statistics/categories/search/object?uri=https://{dspace.url}/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb` for an Item of entity type `Person`:

```json
{
    "page": {
        "number": 0,
        "size": 20,
        "totalPages": 1,
        "totalElements": 3
    },
    "_embedded": {
        "categories": [
            {
                "id": "person-mainReports",
                "type": "category",
                "uniqueType": "statistics.category",
                "category-type": "mainReports",
                "_links": {
                    "self": {
                        "href": "https://{dspace.url}/server/api/statistics/categories/person-mainReports"
                    }
                }
            },
            {
                "id": "person-publicationsReports",
                "type": "category",
                "uniqueType": "statistics.category",
                "category-type": "publicationsReports",
                "_links": {
                    "self": {
                        "href": "https://{dspace.url}/server/api/statistics/categories/person-publicationsReports"
                    }
                }
            },
            {
                "id": "person-projectsReports",
                "type": "category",
                "uniqueType": "statistics.category",
                "category-type": "projectsReports",
                "_links": {
                    "self": {
                        "href": "https://{dspace.url}/server/api/statistics/categories/person-projectsReports"
                    }
                }
            }
        ]
    }
}
```

The categories returned depend on the type of the DSpaceObject (Site, Community, Collection, Item, Bitstream). For Items and Collections an entity-type-specific mapping is used when one exists (based on the `dspace.entity.type` metadata value); otherwise the generic `item` or `collection` categories are returned.

Possible response status:

* 200 OK - The DSpaceObject has been found, and its categories, possibly an empty page, have been returned.
* 400 Bad Request - The `uri` parameter is missing or its format is incorrect, or no DSpaceObject exists for the given UUID.
