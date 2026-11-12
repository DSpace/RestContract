# Login statistics
[Back to the list of all defined endpoints](endpoints.md)

The login statistics endpoints expose, per user, how many times each EPerson has
logged in. The data is read from the Solr statistics core (records with
`statistics_type:login`) and grouped by `epersonid`.

These endpoints are restricted to administrators: all data-returning methods are
protected by `@PreAuthorize("hasAuthority('ADMIN')")` (the `findAll` listing
method is not annotated, since it is not implemented and returns `405`). Clients
should also check the `canViewLoginStatistics` authorization feature exposed on
the `Site` resource
before showing login-statistics navigation, as it is the server-side switch that
tells the UI whether the current user may access this data. At present that
feature simply returns `true` for administrators and `false` for everyone else.

## Single user login statistics
**GET /api/statistics/logins/<:uuid>**

Returns the aggregated login statistics for the EPerson identified by the given
UUID. The UUID in the response is the EPerson UUID, not a separate statistics
identifier.

An example JSON response document to `/api/statistics/logins/1911e8a4-6939-490c-b58b-a5d70f8d91fb`:
```json
{
  "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
  "uniqueType": "statistics.login",
  "type": "login",
  "name": "First User",
  "email": "user1@email.com",
  "count": 3
}
```

Attributes:

- `id`: the UUID of the EPerson whose logins are counted
- `uniqueType`: always `statistics.login`
- `type`: always `login`
- `name`: the full name of the EPerson
- `email`: the email address of the EPerson
- `count`: the number of login events recorded for that EPerson

Possible response status

- 200 OK - The EPerson exists and has at least one recorded login
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 404 Not Found - No EPerson exists with the given UUID, or that EPerson has no recorded login events

## Search login statistics by date range
**GET /api/statistics/logins/search/byDateRange**

Returns a paginated list of login statistics, one entry per EPerson, considering
only the login events that fall within the requested time range. Entries are
ordered by `count` in descending order (users with the most logins first).

The following parameters are supported:

- `startDate`: the lower bound of the range, in `yyyy-MM-dd` format. When omitted, no lower bound is applied
- `endDate`: the upper bound of the range, in `yyyy-MM-dd` format. When omitted, the current date and time is used as the upper bound if `startDate` is present; when both dates are omitted, no date filter is applied
- `page`: the page number
- `size`: the number of entries in a page

Both date bounds are interpreted as the start of the day (`00:00`) in the JVM's
default time zone; that local midnight is then written to the Solr query with a
UTC offset, so the bounds are sent as `00:00Z`. They are inclusive. As a
consequence, passing the same date as both `startDate` and `endDate` selects
almost no events; to cover a whole day, pass the following day as `endDate`.

An example JSON response document to `/api/statistics/logins/search/byDateRange?startDate=2026-10-01&endDate=2026-11-06&page=0&size=2`:
```json
{
  "_embedded": {
    "logins": [
      {
        "id": "8d33bdfb-e7ba-43e6-a93a-f445b7e8a1e2",
        "uniqueType": "statistics.login",
        "type": "login",
        "name": "Second User",
        "email": "user2@email.com",
        "count": 5
      },
      {
        "id": "1911e8a4-6939-490c-b58b-a5d70f8d91fb",
        "uniqueType": "statistics.login",
        "type": "login",
        "name": "First User",
        "email": "user1@email.com",
        "count": 3
      }
    ]
  },
  "page": {
    "size": 2,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

Users without any login event in the range, and login records whose EPerson no
longer exists, are omitted from the result. If no entry matches, `_embedded` is
not present in the response.

Possible response status

- 200 OK - The search was performed, possibly returning an empty result
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 422 Unprocessable Entity - `startDate` or `endDate` is not in the expected `yyyy-MM-dd` format

## All login statistics
**GET /api/statistics/logins**

As there is no use case to iterate over all the existing login statistics
without a date range, the main endpoint is not implemented and a 405 error code
is returned according to our [general error response codes](README.md#Error-codes).
