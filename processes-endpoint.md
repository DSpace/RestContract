# Admin Processes Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint allows adminstrators to list and manipulate script processes created by the [Scripts endpoint](scripts-endpoint.md).

## Execution List
** GET /api/admin/processes**

This endpoint will return a list of all created processes. The JSON response document is as follow:

```
{
  "page": {
      	"size": 5,
      	"totalElements": 14,
      	"totalPages": 3,
      	"number": 0
  },
  "sort" : {
    "by" : "name",
    "order" : "asc"
  },
  "_embedded" : {
    "processes" [
      {
        "processId" : "000003f1-a850-49de-af03-997272d834c9",
        "scriptName" : "import",
        "startTime" : "2017-11-22T10:29:11Z",
        "status" : "RUNNING"
        "_links" : {
          "self" : {
            "href" : "/api/admin/processes/000003f1-a850-49de-af03-997272d834c9"
          }
        }
      },
      {
        "processId" : "d3007ea8-1cc0-481d-914a-06720a200edf",
        "scriptName" : "metadata-export",
        "startTime" : "2017-11-20T10:29:11Z",
        "status" : "FAILED"
        "_links" : {
          "self" : {
            "href" : "/api/admin/processes/000003f1-a850-49de-af03-997272d834c9"
          }
        }
      },
      ...
    ]
  }
```
## Execution Details
** GET /api/admin/processes/<:process-id>**

This endpoint will return details on the requested process.

```
{
  "processId" : "000003f1-a850-49de-af03-997272d834c9",
  "scriptName" : "import",
  "startTime" : "2017-11-22T10:29:11Z",
  "status" : "RUNNING"
  "_links" : {
    "self" : {
      "href" : "/api/admin/processes/000003f1-a850-49de-af03-997272d834c9"
    }
  }
}
```

The possible `status` values are `RUNNING`, `STOPPED`, `FAILED` and `SCHEDULED`.

## Execution Console Output
** GET /api/admin/processes/<:process-id>/output**

This endpoint will return the console output of the script, if any, in plain text.

```
Adding items from directory: /Users/tom/Development/dspaces/dspace7/imports/saf.zip
Generating mapfile: temp
Adding item from directory item_1
	Loading dublin core from /Users/tom/Development/dspaces/dspace7/imports/saf.zip/item_1/dublin_core.xml
	Schema: dc Element: title Qualifier:  Value: Bunny Video
	Processing contents file: /Users/tom/Development/dspaces/dspace7/imports/saf.zip/item_1/contents
	Bitstream: mp4test.mp4
Processing handle file: handle
It appears there is no handle file -- generating one
0 item_1
```

## Execution File Output
** GET /api/admin/processes/<:process-id>/file**

This endpoint will let an administrator download an output file created by a process. If the file is found, it will be presented as a download. This endpoint will support "Range" HTTP headers so that downloads can be paused and resumed.

Parameters:
* `name`: The name of the output file to retrieve

## Execution Deletion
** DELETE /api/admin/processes/<:process-id>**

If the process is still in a `RUNNING` or `SCHEDULED` status, a best-effort attempt will be made to stop the process. Then the process information will be removed.
