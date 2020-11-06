# Displaying usage statistics
[Back to the list of all defined endpoints](endpoints.md)

## Statistics for a DSpaceObject
**GET /api/statistics/usagereports**

As we don't have a use case to iterate over all the existing usage reports, the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error-codes).

## Single statistic
**GET /api/statistics/usagereports/<:id>**

This endpoint provides a specific statistic

An example JSON response document to `/api/statistics/usagereports/1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries`:
```json
{
    "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries",
    "type": "usagereport",
    "report-type": "TopCountries",
    "points": [
        {
            "label": "United States",
            "type": "country",
            "id": "US",
            "values": {
                "views": 2
            }
        },
        {
            "label": "China",
            "type": "country",
            "id": "CN",
            "values": {
                "views": 1
            }
        }
    ]
}
```

The ID is built from a concatenation of the UUID and the type of report for the given UUID

The type of point can be:
* country
* city
* bitstream
* item
* collection
* community
* date

The report-type specifies what data is returned, relevant e.g. to add a label to report. The current statistics support:
* TotalVisits
* TotalVisitsPerMonth
* TotalDownloads
* TopCountries
* TopCities

Possible response status

- 200 OK - The specific statistics data was found, and the data has been properly returned.
* 401 Unauthorized - if you are not authenticated and the statistics are not available to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions and the statistics are not available to the Anonymous user
- 404 Not Found - The specified ID was not found

## Search Statistics for a DSpaceObject
**GET /api/statistics/usagereports/search/object**

This endpoint provides a paginated list of statistics for a DSpaceObject. 

The DSpaceObject is given through the following parameters:
- `uri` The object to retrieve statistics for. The full URI of the rest resource must be specified, i.e. https://{dspace.url}/server/api/core/community/{uuid}

The usual parameters for paginated lists are supported as well:
- `page` The page number 
- `size` The number of reports in a page

An example JSON response document to `/api/statistics/usagereports/search/object?page=0&size=2&uri=https://{dspace.url}/server/api/core/site/6d65c6a2-3fe7-44dd-bacb-79271257c35d`:

```json
{
    "_embedded": {
        "usagereports": [
            {
                "id": "6d65c6a2-3fe7-44dd-bacb-79271257c35d_TotalVisits",
                "type": "usagereport",
                "report-type": "TotalVisits",
                "points": [
                    {
                        "label": "cad835c8-0cae-4769-a08a-857f0f814020",
                        "type": "item",
                        "id": "cad835c8-0cae-4769-a08a-857f0f814020",
                        "values": {
                            "views": 3313
                        }
                    },
                    {
                        "label": "6759d5e0-3915-4864-917c-1940bdb75fbd",
                        "type": "item",
                        "id": "6759d5e0-3915-4864-917c-1940bdb75fbd",
                        "values": {
                            "views": 3308
                        }
                    },
                    {
                        "label": "b0f6ce54-2ed8-4b67-a075-64794abb4e82",
                        "type": "item",
                        "id": "6759d5e0-3915-4864-917c-1940bdb75fbd",
                        "values": {
                            "views": 1800
                        }
                    }
                ]
            }
        ]
    }
}
```

An example JSON response document to `/api/statistics/usagereports/search/object?uri=https://{dspace.url}/server/api/core/item/1911e8a4-6939-490c-b58b-a5d70f8d91fb`:

```json
{
    "_embedded": {
        "usagereports": [
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisits",
                "type": "usagereport",
                "report-type": "TotalVisits",
                "points": [
                    {
                        "label": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
                        "type": "item",
                        "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
                        "values": {
                            "views": 3
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalVisitsPerMonth",
                "type": "usagereport",
                "report-type": "TotalVisitsPerMonth",
                "points": [
                    {
                        "label": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
                        "type": "date",
                        "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
                        "values": {
                            "2020-03": 0,
                            "2020-04": 0,
                            "2020-05": 3
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TotalDownloads",
                "type": "usagereport",
                "report-type": "TotalDownloads",
                "points": [
                    {
                        "label": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
                        "type": "bitstream",
                        "id": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
                        "values": {
                            "downloads": 8
                        }
                    }
                ]
            },
            {
                "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb_TopCountries",
                "type": "usagereport",
                "report-type": "TopCountries",
                "points": [
                    {
                        "label": "United States",
                        "type": "country",
                        "id": "US",
                        "values": {
                            "views": 2
                        }
                    },
                    {
                        "label": "China",
                        "type": "country",
                        "id": "CN",
                        "values": {
                            "views": 1
                        }
                    }
                ]
            }
        ]
    }
}
```

Possible response status:
* 200 OK - The DSpaceObject was found, and the data has been properly returned.
* 400 Bad Request - The uri parameter format is incorrect
* 401 Unauthorized - if you are not authenticated and the statistics are not available to the Anonymous user
* 403 Forbidden - if you are not logged in with sufficient permissions and the statistics are not available to the Anonymous user
