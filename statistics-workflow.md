# Workflow statistics
[Back to the list of all defined endpoints](endpoints.md)

DSpace records a workflow statistic every time a workflow action is performed.
These records live in the Solr statistics core with `statistics_type:workflow`
and only actions that require a UI interaction (`previousActionRequiresUI: true`)
are taken into account. The records are used by two families of read-only
endpoints:

* **workflow owner statistics** (`/api/statistics/workflowOwners`), which aggregate
  the actions performed by each EPerson;
* **workflow step statistics** (`/api/statistics/wfSteps`), which aggregate the
  actions performed in each workflow step.

Both endpoints are restricted to administrators: all data-returning methods are
protected by `@PreAuthorize("hasAuthority('ADMIN')")` (the `findAll` listing
methods are not annotated, since they are not implemented and return `405`). A
request without authentication gets `401 Unauthorized`; an authenticated user
without the `ADMIN` authority gets `403 Forbidden`.

The UI also exposes the `canViewWorkflowStatistics` authorization feature on
`Site`, `Community` and `Collection` resources to decide whether workflow
statistics navigation should be shown. That feature is controlled by the
`usage-statistics.authorization.admin.workflow` configuration property, but it
does **not** grant access to the REST endpoints below: those always require the
`ADMIN` authority.

## Workflow owner statistics
**GET /api/statistics/workflowOwners**

As there is no use case to iterate over all the workflow owners without a date
range, the main endpoint is not implemented and a `405` error code is returned
according to our [general error response codes](README.md#Error-codes).

## Single workflow owner statistics
**GET /api/statistics/workflowOwners/<:uuid>**

Returns the aggregated workflow statistics for the EPerson identified by the
given UUID. The UUID is the EPerson UUID, not a separate statistics identifier.

An example JSON response document to `/api/statistics/workflowOwners/8e1f6e30-1d1e-4d63-9a3f-2a6f0e0b9c11`:
```json
{
  "id": "8e1f6e30-1d1e-4d63-9a3f-2a6f0e0b9c11",
  "type": "workflowOwner",
  "uniqueType": "statistics.workflowOwner",
  "name": "First User",
  "email": "user1@email.com",
  "count": 6,
  "actionCounts": {
    "reviewstep.claimaction": 2,
    "reviewstep.reviewaction": 2,
    "editstep.claimaction": 1,
    "editstep.editaction": 1
  },
  "_links": {
    "self": {
      "href": "https://{dspace.url}/server/api/statistics/workflowOwners/8e1f6e30-1d1e-4d63-9a3f-2a6f0e0b9c11"
    }
  }
}
```

Attributes:

- `id`: the UUID of the EPerson whose actions are counted
- `type`: always `workflowOwner`
- `uniqueType`: always `statistics.workflowOwner`
- `name`: the full name of the EPerson
- `email`: the email address of the EPerson
- `count`: the total number of workflow actions attributed to the EPerson
- `actionCounts`: a map whose keys have the form `<step>.<action>` (for example
  `reviewstep.claimaction`) and whose values are the number of times the EPerson
  performed that action in that step. The sum of the values equals `count`

Actions listed in `statistics.workflow.actions-to-filter` are excluded from both
`count` and `actionCounts` (see [Configuration](#configuration)).

> **Note:** the single-owner query does not exclude actions whose previous step is
> `submit`, while the owner *search* endpoint does (see below). As a consequence,
> a submitter can have a higher `count` here than in the date-range listing.

Possible response status

- 200 OK - The EPerson exists and has at least one recorded workflow action
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 404 Not Found - No EPerson exists with the given UUID, or that EPerson has no recorded workflow actions

## Search workflow owner statistics by date range
**GET /api/statistics/workflowOwners/search/byDateRange**

Returns a list of workflow owner statistics, one entry per EPerson, considering
only the workflow actions that fall within the requested time range. Actions
whose previous step is `submit` are not attributed to an owner by this endpoint.
Entries are ordered by `count` in descending order (owners with the most actions
first); entries with the same count have no guaranteed relative order.

The following parameters are supported:

- `startDate`: the lower bound of the range, in `yyyy-MM-dd` format. When omitted, no lower bound is applied
- `endDate`: the upper bound of the range, in `yyyy-MM-dd` format. When omitted, the current date and time is used as the upper bound if `startDate` is present; when both dates are omitted, no date filter is applied
- `collection`: an optional Collection UUID used to restrict the statistics to workflow items belonging to that Collection
- `page`: the page number
- `size`: the number of entries in a page

Both date bounds are taken at the start of the day (`00:00`) in the JVM's default
time zone; that local midnight is then written to the Solr query with a UTC
offset, so the bounds are sent as `00:00Z`. The range starts at
`00:00` of `startDate` and ends at `00:00` of `endDate`, so to cover a whole
calendar day you should pass the following day as `endDate`.

If a non-blank `collection` parameter is supplied but no Collection exists with
that UUID (including a value that is not a valid UUID), the request fails with
`400 Bad Request`.

An example JSON response document to `/api/statistics/workflowOwners/search/byDateRange?startDate=2026-10-01&endDate=2026-11-06&page=0&size=2`:
```json
{
  "_embedded": {
    "workflowOwners": [
      {
        "id": "8e1f6e30-1d1e-4d63-9a3f-2a6f0e0b9c11",
        "type": "workflowOwner",
        "uniqueType": "statistics.workflowOwner",
        "name": "First User",
        "email": "user1@email.com",
        "count": 6,
        "actionCounts": {
          "reviewstep.claimaction": 2,
          "reviewstep.reviewaction": 2,
          "editstep.claimaction": 1,
          "editstep.editaction": 1
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/workflowOwners/8e1f6e30-1d1e-4d63-9a3f-2a6f0e0b9c11"
          }
        }
      },
      {
        "id": "c0b6b0a4-3a5e-4f0f-9f0e-6f1c2d3e4f50",
        "type": "workflowOwner",
        "uniqueType": "statistics.workflowOwner",
        "name": "Third User",
        "email": "user3@email.com",
        "count": 4,
        "actionCounts": {
          "editstep.claimaction": 2,
          "editstep.editaction": 2
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/workflowOwners/c0b6b0a4-3a5e-4f0f-9f0e-6f1c2d3e4f50"
          }
        }
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

Owners whose EPerson no longer exists, and owners without any workflow action in
the range, are omitted from the result. If no entry matches, `_embedded` is not
present in the response.

Possible response status

- 200 OK - The search was performed, possibly returning an empty result
- 400 Bad Request - The `collection` parameter does not resolve to an existing Collection
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 422 Unprocessable Entity - `startDate` or `endDate` is not in the expected `yyyy-MM-dd` format

## Workflow step statistics
**GET /api/statistics/wfSteps**

As there is no use case to iterate over all the workflow steps without a date
range, the main endpoint is not implemented and a `405` error code is returned
according to our [general error response codes](README.md#Error-codes).

`wfSteps` is the abbreviated endpoint segment for workflow steps; the REST type
is `workflowStep` and, when the steps appear in a paginated response, they are
embedded under the `workflowSteps` key.

## Single workflow step statistics
**GET /api/statistics/wfSteps/<:id>**

Returns the aggregated workflow statistics for the step identified by the given
name. **The identifier is the workflow step name, not a UUID** — for example
`submit`, `reviewstep`, `editstep`, `item` or `workspace`. The names available
depend on the workflow configuration and on the statistics that have been
recorded.

The `item` and `workspace` steps are final/terminal states and are counted from
the step reached by the item, while the other steps are counted from the step
that was left when the action was performed.

An example JSON response document to `/api/statistics/wfSteps/reviewstep`:
```json
{
  "id": "reviewstep",
  "type": "workflowStep",
  "uniqueType": "statistics.workflowStep",
  "name": "reviewstep",
  "count": 6,
  "actionCounts": {
    "claimaction": 3,
    "reviewaction": 3
  },
  "_links": {
    "self": {
      "href": "https://{dspace.url}/server/api/statistics/wfSteps/reviewstep"
    }
  }
}
```

Attributes:

- `id`: the step name
- `type`: always `workflowStep`
- `uniqueType`: always `statistics.workflowStep`
- `name`: the step name (same value as `id`)
- `count`: the total number of actions recorded for the step
- `actionCounts`: a map whose keys are action names (for example `claimaction`,
  `reviewaction`, `editaction`, `approve`, `reject`) and whose values are the
  number of times each action was performed in that step; the sum of the values
  equals `count`

When there are no action counts, the `actionCounts` attribute is omitted from the
response rather than being rendered as an empty object.

Actions listed in `statistics.workflow.actions-to-filter` are excluded from both
`count` and `actionCounts` (see [Configuration](#configuration)).

Possible response status

- 200 OK - The step has at least one recorded workflow action
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 404 Not Found - No statistics exist for the given step name

## Search workflow step statistics by date range
**GET /api/statistics/wfSteps/search/byDateRange**

Returns a list of workflow step statistics, one entry per step, considering only
the workflow actions that fall within the requested time range. Entries are
ordered by `count` in descending order; entries with the same count have no
guaranteed relative order.

The following parameters are supported:

- `startDate`: the lower bound of the range, in `yyyy-MM-dd` format. When omitted, no lower bound is applied
- `endDate`: the upper bound of the range, in `yyyy-MM-dd` format. When omitted, the current date and time is used as the upper bound if `startDate` is present; when both dates are omitted, no date filter is applied
- `collection`: an optional Collection UUID used to restrict the statistics to workflow items belonging to that Collection
- `page`: the page number
- `size`: the number of entries in a page

The result merges the statistics of the regular steps (including `submit`) with
those of the `item` and `workspace` steps, orders the merged list by `count` in
descending order and then applies the `size` limit. Steps whose count is zero are
not returned.

Both date bounds are taken at the start of the day (`00:00`) in the JVM's default
time zone; that local midnight is then written to the Solr query with a UTC
offset, so the bounds are sent as `00:00Z`. The range starts at
`00:00` of `startDate` and ends at `00:00` of `endDate`, so to cover a whole
calendar day you should pass the following day as `endDate`.

If a non-blank `collection` parameter is supplied but no Collection exists with
that UUID (including a value that is not a valid UUID), the request fails with
`400 Bad Request`.

An example JSON response document to `/api/statistics/wfSteps/search/byDateRange?startDate=2026-10-01&endDate=2026-11-06&page=0&size=2`:
```json
{
  "_embedded": {
    "workflowSteps": [
      {
        "id": "reviewstep",
        "type": "workflowStep",
        "uniqueType": "statistics.workflowStep",
        "name": "reviewstep",
        "count": 5,
        "actionCounts": {
          "claimaction": 3,
          "reviewaction": 2
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/wfSteps/reviewstep"
          }
        }
      },
      {
        "id": "submit",
        "type": "workflowStep",
        "uniqueType": "statistics.workflowStep",
        "name": "submit",
        "count": 4,
        "actionCounts": {
          "submit": 4
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/wfSteps/submit"
          }
        }
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

If no entry matches, `_embedded` is not present in the response.

Possible response status

- 200 OK - The search was performed, possibly returning an empty result
- 400 Bad Request - The `collection` parameter does not resolve to an existing Collection
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator
- 422 Unprocessable Entity - `startDate` or `endDate` is not in the expected `yyyy-MM-dd` format

## Current workflow step statistics
**GET /api/statistics/wfSteps/search/current**

Returns, for each workflow step, the number of workflow items currently waiting
in that step, grouped by the action associated with the pending task (for example
`claimaction` for a pooled task that has not been claimed yet). Unlike the
date-range endpoints, these are in-progress counts rather than historical action
counts: the data comes from the claimed and pooled tasks currently present in the
workflow, not from the Solr statistics core.

The following parameters are supported:

- `page`: the page number
- `size`: the number of steps in a page

Entries are ordered by `count` in descending order; entries with the same count
have no guaranteed relative order.

An example JSON response document to `/api/statistics/wfSteps/search/current?page=0&size=20`:
```json
{
  "_embedded": {
    "workflowSteps": [
      {
        "id": "reviewstep",
        "type": "workflowStep",
        "uniqueType": "statistics.workflowStep",
        "name": "reviewstep",
        "count": 2,
        "actionCounts": {
          "claimaction": 2
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/wfSteps/reviewstep"
          }
        }
      },
      {
        "id": "editstep",
        "type": "workflowStep",
        "uniqueType": "statistics.workflowStep",
        "name": "editstep",
        "count": 1,
        "actionCounts": {
          "claimaction": 1
        },
        "_links": {
          "self": {
            "href": "https://{dspace.url}/server/api/statistics/wfSteps/editstep"
          }
        }
      }
    ]
  },
  "page": {
    "size": 20,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  }
}
```

When no workflow item is currently in progress, `_embedded` is not present in the
response.

Possible response status

- 200 OK - The search was performed, possibly returning an empty result
- 401 Unauthorized - The request is not authenticated
- 403 Forbidden - The authenticated user is not an administrator

## Configuration

### `statistics.workflow.actions-to-filter`
A comma-separated list of workflow action names to exclude from all workflow
statistics. The default is empty, so no action is excluded. For example, to
ignore claim actions:

```
statistics.workflow.actions-to-filter = claimaction
```

The exclusion is applied to the action that was performed (the
`previousWorkflowAction` field), so the filtered actions disappear both from
`count` and from `actionCounts`, and `count` remains equal to the sum of the
remaining `actionCounts` values.

### `usage-statistics.authorization.admin.workflow`
Controls the `canViewWorkflowStatistics` authorization feature used by the UI to
decide whether to display workflow statistics. It does not affect the REST
endpoints, which always require the `ADMIN` authority.
