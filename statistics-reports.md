# Displaying usage statistics
[Back to the list of all defined endpoints](endpoints.md)

## Statistics for a DSpaceObject
**GET /api/statistics/usagereports**

As we don't have a use case to iterate over all the existing usage reports, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Single statistic
**GET /api/statistics/usagereports/<:id>**

This endpoint provides a specific statistic.

The ID is built from a concatenation of the UUID of the DSpaceObject and the identifier of the report, separated by an underscore: `<uuid>_<report-id>`, e.g. `1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries`. The report identifiers are configured in `dspace/config/spring/rest/statistics.xml` (see [Report identifiers and types](#report-identifiers-and-types)).

An example JSON response document to `/api/statistics/usagereports/1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries`:
```json
{
    "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries",
    "type": "usagereport",
    "uniqueType": "statistics.usagereport",
    "report-type": "TopCountries",
    "view-mode": "map",
    "points": [
        {
            "id": "US",
            "label": "United States",
            "type": "country",
            "uniqueType": "statistics.country",
            "values": {
                "views": 2
            }
        },
        {
            "id": "CN",
            "label": "China",
            "type": "country",
            "uniqueType": "statistics.country",
            "values": {
                "views": 1
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://{dspace.url}/server/api/statistics/usagereports/1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries"
        }
    }
}
```

Attributes:

* `id`: the `<uuid>_<report-id>` identifier of the report
* `type`: always `usagereport`
* `uniqueType`: always `statistics.usagereport`
* `report-type`: the base type of the generator that produced the report, useful e.g. to choose a label or an icon for it. It is not necessarily equal to the report identifier (see [Report identifiers and types](#report-identifiers-and-types))
* `view-mode`: a suggestion for the client about how to visualise the report. Possible values are `table`, `map`, `chart.line` and `chart.pie`
* `points`: the list of data points of the report. It can be empty when no data is available
* `_links.self.href`: the link to this report

Each point has the following attributes:

* `id`: the identifier of the point, e.g. the ISO 3166-1 alpha-2 country code for country points, or the period label for date points
* `label`: the human-readable label of the point
* `type`: the type of the point (see below)
* `uniqueType`: the string `statistics.<type>`
* `values`: a map of measures. The key is always `views`, also for reports about downloads (e.g. `TotalDownloads`): the number of download events is returned under the `views` key as well

The type of point can be:

| type | uniqueType | `id` | `label` |
|---|---|---|---|
| `country` | `statistics.country` | ISO 3166-1 alpha-2 code, e.g. `US` | country name, localised |
| `continent` | `statistics.continent` | continent name | continent name |
| `city` | `statistics.city` | city name | city name |
| `category` | `statistics.category` | category key, e.g. `article` | category key |
| `date` | `statistics.date` | period label, e.g. `September 2020` for a monthly report | same as `id` |
| `item` | `statistics.item` | UUID of the item | item title, or the DSO name for a total report on the item |
| `collection` | `statistics.collection` | UUID of the collection | collection name |
| `community` | `statistics.community` | UUID of the community | community name |
| `bitstream` | `statistics.bitstream` | UUID of the bitstream | bitstream name |
| `site` | `statistics.site` | UUID of the site | site name |

The `date` points of a monthly report use the server locale to format the period, e.g. `September 2020`; the same value is used for both `id` and `label`.

Possible response status

- 200 OK - The specific statistics data was found, and the data has been properly returned.
* 401 Unauthorized - if you are not authenticated and the statistics are not available to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions and the statistics are not available to the Anonymous user
- 404 Not Found - The specified ID was not found, or it is not a valid `<uuid>_<report-id>` identifier

By default, any user who can READ the DSpaceObject can also read its statistics. Setting the `usage-statistics.authorization.admin.usage` configuration property to `true` restricts them to administrators.

## Search Statistics for a DSpaceObject
**GET /api/statistics/usagereports/search/object**

This endpoint provides a paginated list of statistics for a DSpaceObject.

The DSpaceObject is given through the following parameters:
- `uri` (**required**): the object to retrieve statistics for. The full URI of the REST resource must be specified, i.e. `https://{dspace.url}/server/api/core/communities/{uuid}`. The UUID is extracted from the last path segment
- `category` (optional): when present, only the reports belonging to the requested category are returned. The value must be the `id` of a category applicable to the object, see [Statistics Categories](statistics-categories.md). A category that does not apply to the given object results in a 400 response
- `startDate` (optional): the date from which the statistics are to be taken into account, in `YYYY-MM-DD` format
- `endDate` (optional): the date until which the statistics are to be taken into account, in `YYYY-MM-DD` format. Both bounds are inclusive. When `endDate` is omitted no upper bound is applied; the reports that aggregate over a period use the current date as the end of the range

For the reports that aggregate data over a period (e.g. `TotalVisitsPerMonth`), the range also determines the periods reported: when `startDate` is supplied, every period in the range is returned, including the ones with zero views; when it is omitted, the report starts from the first period that has data and leading empty periods are trimmed.

The usual parameters for paginated lists are supported as well:
- `page` The page number
- `size` The number of reports in a page
- `projection` (optional): `excludeUsageReportPoints` returns only the report headers. When this projection is used the `points` attribute is present as an empty array and the points are not calculated, which improves performance when only the available reports are needed

An example JSON response document to `/api/statistics/usagereports/search/object?uri=https://{dspace.url}/server/api/core/items/1911e8a4-6939-490c-b58b-a5d70f8d91fb`:

```json
{
    "_embedded": {
        "usagereports": [
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisits",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TotalVisits",
                "view-mode": "table",
                "points": [
                    {
                        "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
                        "label": "My item",
                        "type": "item",
                        "uniqueType": "statistics.item",
                        "values": {
                            "views": 3
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TotalVisitsPerMonth",
                "view-mode": "chart.line",
                "points": [
                    {
                        "id": "March 2020",
                        "label": "March 2020",
                        "type": "date",
                        "uniqueType": "statistics.date",
                        "values": {
                            "views": 0
                        }
                    },
                    {
                        "id": "April 2020",
                        "label": "April 2020",
                        "type": "date",
                        "uniqueType": "statistics.date",
                        "values": {
                            "views": 0
                        }
                    },
                    {
                        "id": "May 2020",
                        "label": "May 2020",
                        "type": "date",
                        "uniqueType": "statistics.date",
                        "values": {
                            "views": 3
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TopCountries",
                "view-mode": "map",
                "points": [
                    {
                        "id": "US",
                        "label": "United States",
                        "type": "country",
                        "uniqueType": "statistics.country",
                        "values": {
                            "views": 2
                        }
                    },
                    {
                        "id": "CN",
                        "label": "China",
                        "type": "country",
                        "uniqueType": "statistics.country",
                        "values": {
                            "views": 1
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCities",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TopCities",
                "view-mode": "table",
                "points": []
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalDownloads",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TotalDownloads",
                "view-mode": "table",
                "points": [
                    {
                        "id": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
                        "label": "bitstream.pdf",
                        "type": "bitstream",
                        "uniqueType": "statistics.bitstream",
                        "values": {
                            "views": 8
                        }
                    }
                ]
            }
        ]
    },
    "page": {
        "number": 0,
        "size": 20,
        "totalPages": 1,
        "totalElements": 5
    }
}
```

The same request with `&page=0&size=2&projection=excludeUsageReportPoints` returns the report headers with an empty `points` array:

```json
{
    "_embedded": {
        "usagereports": [
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisits",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TotalVisits",
                "view-mode": "table",
                "points": []
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TotalVisitsPerMonth",
                "view-mode": "chart.line",
                "points": []
            }
        ]
    },
    "page": {
        "number": 0,
        "size": 2,
        "totalPages": 3,
        "totalElements": 5
    }
}
```

An example JSON response document to `/api/statistics/usagereports/search/object?uri=https://{dspace.url}/server/api/core/sites/6d65c6a2-3fe7-44dd-bacb-79271257c35d&category=site-mainReports&startDate=2020-01-01&endDate=2020-05-31&size=1`. The reports of the category are returned in the order in which they are configured in `statistics.xml`, so with `size=1` only the first one, `TopContinents`, is included:

```json
{
    "_embedded": {
        "usagereports": [
            {
                "id": "6d65c6a2-3fe7-44dd-bacb-79271257c35d_TopContinents",
                "type": "usagereport",
                "uniqueType": "statistics.usagereport",
                "report-type": "TopContinents",
                "view-mode": "table",
                "points": [
                    {
                        "id": "Europe",
                        "label": "Europe",
                        "type": "continent",
                        "uniqueType": "statistics.continent",
                        "values": {
                            "views": 3313
                        }
                    },
                    {
                        "id": "North America",
                        "label": "North America",
                        "type": "continent",
                        "uniqueType": "statistics.continent",
                        "values": {
                            "views": 1284
                        }
                    }
                ]
            }
        ]
    },
    "page": {
        "number": 0,
        "size": 1,
        "totalPages": 6,
        "totalElements": 6
    }
}
```

The categories applicable to an object depend on its type and, for items and collections, on its entity type. See [Statistics Categories](statistics-categories.md) for the full list of identifiers (`site-mainReports`, `site-downloadReports`, `community-mainReports`, `community-itemReports`, `community-downloadReports`, `collection-mainReports`, `collection-itemReports`, `collection-downloadReports`, `item-mainReports`, `bitstream-mainReports`, and the entity-specific ones such as `person-mainReports`, `publication-mainReports`, `project-mainReports`, `affiliation-mainReports`).

Possible response status:

* 200 OK - The DSpaceObject was found, and the data has been properly returned.
* 400 Bad Request - The `uri` parameter is missing or does not end with a valid UUID, or the requested `category` does not apply to the given DSpaceObject
* 404 Not Found - The DSpaceObject identified by the `uri` was not found

## Search a single statistic over a period
**GET /api/statistics/usagereports/search/report**

This endpoint returns the same representation as the [single statistic](#single-statistic) endpoint, restricted to an optional date range. It is useful when the client already knows which report it needs and does not want to retrieve all the reports of the object.

The report is identified through the following parameters:
- `id` (**required**): the identifier of the report, in the `<uuid>_<report-id>` form (see [Single statistic](#single-statistic))
- `startDate` (optional): the date from which the statistics are to be taken into account, in `YYYY-MM-DD` format
- `endDate` (optional): the date until which the statistics are to be taken into account, in `YYYY-MM-DD` format. When omitted no upper bound is applied; the reports that aggregate over a period use the current date as the end of the range

An example JSON response document to `/api/statistics/usagereports/search/report?id=1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth&startDate=2020-03-01&endDate=2020-05-31`:
```json
{
    "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth",
    "type": "usagereport",
    "uniqueType": "statistics.usagereport",
    "report-type": "TotalVisitsPerMonth",
    "view-mode": "chart.line",
    "points": [
        {
            "id": "March 2020",
            "label": "March 2020",
            "type": "date",
            "uniqueType": "statistics.date",
            "values": {
                "views": 0
            }
        },
        {
            "id": "April 2020",
            "label": "April 2020",
            "type": "date",
            "uniqueType": "statistics.date",
            "values": {
                "views": 1
            }
        },
        {
            "id": "May 2020",
            "label": "May 2020",
            "type": "date",
            "uniqueType": "statistics.date",
            "values": {
                "views": 3
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://{dspace.url}/server/api/statistics/usagereports/search/report?id=1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth&startDate=2020-03-01&endDate=2020-05-31"
        }
    }
}
```

When no data is available for the requested period, the report is still returned: the periods in the range are reported with zero `views` (when the generator has no data it returns a single zero-valued point for the end period, not an empty array).

Possible response status:

* 200 OK - The report has been properly returned
* 400 Bad Request - The `id` parameter is missing or is not a valid `<uuid>_<report-id>` identifier
* 401 Unauthorized - if you are not authenticated and the statistics are not available to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions and the statistics are not available to the Anonymous user
* 404 Not Found - No DSpaceObject exists with the given UUID, or the report identifier cannot be resolved for that object

## Report identifiers and types

The `report-type` attribute is the base type of the generator that produced the report. The identifiers used in the `<uuid>_<report-id>` `id` are configured in `dspace/config/spring/rest/statistics.xml`, where the same generator can back multiple report identifiers. As a consequence, `report-type` does not necessarily match the suffix of `id`: for example, the `TotalItemsVisits` report has `report-type` `TopItems`, and the `TotalDownloadsPerMonth` report has `report-type` `TotalVisitsPerMonth` because both are produced by the period generator.

The following reports are defined for the standard DSpace objects. Their availability depends on the type of the object and on its category; see [Statistics Categories](statistics-categories.md).

| report-id | report-type | view-mode | Notes |
|---|---|---|---|
| `TotalVisits` | `TotalVisits` | `table` | total views of the object. For the site it is produced by the top-items generator (`report-type` `TopItems`) and lists the most visited items |
| `TotalVisitsPerMonth` | `TotalVisitsPerMonth` | `chart.line` | views per month |
| `TopCountries` | `TopCountries` | `map` | views by country |
| `TopCities` | `TopCities` | `table` | views by city |
| `TopContinents` | `TopContinents` | `table` | views by continent (site main reports) |
| `TopCategories` | `TopCategories` | `chart.pie` | views by entity-type category (site main reports only) |
| `TotalDownloads` | `TotalDownloads` | `table` | downloads of the bitstreams of an item, or of a bitstream. For the site, a community or a collection it is produced by the top-items generator (`report-type` `TopItems`) and lists the most downloaded items |
| `TotalDownloadsPerMonth` | `TotalVisitsPerMonth` | `chart.line` | downloads per month (download reports of site, community and collection) |
| `TotalItemsVisits` | `TopItems` | `table` | most visited items in a collection or community |
| `TotalItemsVisitsPerMonth` | `TotalVisitsPerMonth` | `chart.line` | views per month of the items in a collection or community |
| `TopItemsCountries` | `TopCountries` | `map` | views by country of the items in a collection or community |
| `TopItemsCities` | `TopCities` | `table` | views by city of the items in a collection or community |
| `TopItemsContinents` | `TopContinents` | `table` | views by continent of the items in a collection or community |
| `TopItemsCategories` | `TopCategories` | `chart.pie` | views by entity-type category of the items in a collection or community |
| `TopDownloadsCountries` | `TopCountries` | `map` | downloads by country |
| `TopDownloadsCities` | `TopCities` | `table` | downloads by city |
| `TopDownloadsContinents` | `TopContinents` | `table` | downloads by continent |

When the reported object is related to other objects (for example the research outputs of a Person or an OrgUnit), additional reports are configured. Their identifiers are composed of the base report name and the relation suffix:

| relation | report-ids (and their report-types) |
|---|---|
| Person → research outputs | `TotalVisitsPersonResearchoutputs` (`TotalVisits`), `TotalVisitPerPeriodPersonResearchoutputs` (`TotalVisitsPerMonth`), `TopCountriesPersonResearchoutputs` (`TopCountries`), `TopCitiesPersonResearchoutputs` (`TopCities`) |
| Person → projects | `TotalVisitsPersonProjects` (`TotalVisits`), `TotalVisitPerPeriodPersonProjects` (`TotalVisitsPerMonth`), `TopCountriesPersonProjects` (`TopCountries`), `TopCitiesPersonProjects` (`TopCities`) |
| OrgUnit → research outputs | `TotalVisitsOrgUnitRppublications` (`TotalVisits`), `TotalVisitPerPeriodOrgUnitRppublications` (`TotalVisitsPerMonth`), `TopCountriesOrgUnitRppublications` (`TopCountries`), `TopCitiesOrgUnitRppublications` (`TopCities`), `TopItemsOrgUnitRppublications` (`TopItems`), `TotalDownloadsOrgUnitRppublications` (`TotalDownloads`), `TotalViewsDownloadsOrgUnitRppublications` (`TotalVisitsAndDownloads`) |
| OrgUnit → projects | `TotalVisitsOrgUnitRpprojects` (`TotalVisits`), `TotalVisitPerPeriodOrgUnitRpprojects` (`TotalVisitsPerMonth`), `TopCountriesOrgUnitRpprojects` (`TopCountries`), `TopCitiesOrgUnitRpprojects` (`TopCities`), `TopItemsOrgUnitRpprojects` (`TopItems`), `TotalDownloadsOrgUnitRpprojects` (`TotalDownloads`), `TotalViewsDownloadsOrgUnitRpprojects` (`TotalVisitsAndDownloads`) |
| OrgUnit → publications | `TotalVisitsOrgUnitPublications` (`TotalVisits`), `TotalVisitPerPeriodOrgUnitPublications` (`TotalVisitsPerMonth`), `TopCountriesOrgUnitPublications` (`TopCountries`), `TopCitiesOrgUnitPublications` (`TopCities`), `TopItemsOrgUnitPublications` (`TopItems`), `TotalDownloadsOrgUnitPublications` (`TotalDownloads`), `TotalViewsDownloadsOrgUnitPublications` (`TotalVisitsAndDownloads`) |

The default configuration of the `Project → research outputs` category currently exposes only the `TotalVisits` report.

The `TotalViewsDownloads*` reports return two points, labelled `Item visits` and `File visits`, with their counts under the `views` key.

These lists are not fixed: administrators can add or remove report identifiers and change their `view-mode` in `statistics.xml` without changing the REST API.
