# Admin Processes Endpoints
[Back to the list of all defined endpoints](endpoints.md)

This endpoint allows users to list and manipulate script processes created by the [Scripts endpoint](scripts-endpoint.md).

## Execution List
**GET /api/system/processes**

This endpoint will return a list of all created processes. The JSON response document is as follows:

```json
{
  "page": {
      	"size": 5,
      	"totalElements": 14,
      	"totalPages": 3,
      	"number": 0
  },
  "_embedded" : {
    "processes" : [
      {
        "processId" : "1",
        "userId" : "aa0263e2-b90a-4528-89fa-116ea4859de1",
        "startTime" : "2017-11-22T10:29:11Z",
        "endTime" : null,
        "scriptName": "metadata-import",
        "processStatus" : "RUNNING",
        "parameters" : [
          {
            "name" : "-c",
            "value" : "954e5cfa-6990-4c85-ae42-f30d8c7888e2"
          },
          {
            "name" : "-n",
            "value" : "true"
          }
        ],
        "_links" : {
          "self" : {
            "href" : "/api/system/processes/1"
          },
          "script" : {
            "href" : "/api/system/scripts/import"
          },
          "output" : {
            "href" : "/api/system/processes/1/output"
          },
          "files" : {
            "href" : "/api/system/processes/1/files"
          }
        }
      },
      {
        "processId" : "2",
        "userId" : "c7d85e7f-63e5-4bc0-96cb-5d80be48d62e",
        "startTime" : "2017-11-20T10:29:11Z",
        "endTime" : "2017-11-20T10:30:11Z",
        "scriptName": "metadata-import",
        "processStatus" : "FAILED",
        "parameters" : [
          {
            "name" : "-i",
            "value" : "c70893a6-ac55-48c7-9447-61e026b62929"
          }
        ],
        "_links" : {
          "self" : {
            "href" : "/api/system/processes/2"
          },
          "script" : {
            "href" : "/api/system/scripts/metadata-export"
          },
          "output" : {
            "href" : "/api/system/processes/2/output"
          },
          "files" : {
            "href" : "/api/system/processes/2/files"
          }
        }
      }
    ]
  }
}
```

Optional parameters to query the processes:
* `script`: The name of the script (e.g. import)
* `userId`: The UUID of the person who created the process (only admins can retrieve processes they didn't start)
* `status`: The status of the script
* `parameter.xyz`: Which parameters have been used, and their value

## Execution Details
**GET /api/system/processes/<:process-id>**

This endpoint will return details on the requested process.

```json
{
  "processId" : "3",
  "userId" : "aa0263e2-b90a-4528-89fa-116ea4859de1",
  "startTime" : "2017-11-22T10:29:11Z",
  "endTime" : null,              
  "scriptName": "metadata-import",
  "processStatus" : "RUNNING",
  "parameters" : [
    {
    "name" : "-c",
    "value" : "954e5cfa-6990-4c85-ae42-f30d8c7888e2"
    },
    {
    "name" : "-n",
    "value" : "true"
    }
  ],
  "_links" : {
    "self" : {
      "href" : "/api/system/processes/3"
    },
    "script" : {
      "href" : "/api/system/scripts/import"
    },
    "output" : {
      "href" : "/api/system/processes/3/output"
    },
    "files" : {
      "href" : "/api/system/processes/3/files"
    }
  }
}
```

The possible `status` values are `RUNNING`, `COMPLETED` and `FAILED`.

## Execution Console Output
**GET /api/system/processes/<:process-id>/output**

This endpoint will return the console output of the script. To keep the scope of this work limited, it will print the full console output without datestamps.
In a future version, a solution supporting a live-feed can also be included.

## Execution File Output List
**GET /api/system/processes/<:process-id>/files**

This endpoint will let an administrator download an output file created by a process. If the file is found, it will be presented as a download. This endpoint will support "Range" HTTP headers so that downloads can be paused and resumed.

```json
{
  "processId" : "4",
  "_links" : {
    "self" : {
      "href" : "/api/system/processes/4/files"
    },
    "mapfile" : {
      "href" : "/api/system/processes/4/files/mapfile"
    },
    "zipfile" : {
      "href" : "/api/system/processes/4/files/zipfile"
    }
  }
}
```

## Execution File Output
**GET /api/system/processes/<:process-id>/files/<:file-name>**

This endpoint will let an administrator download an output file created by a process. If the file is found, it will be presented as a download. This endpoint will support "Range" HTTP headers so that downloads can be paused and resumed.

Required parameters:
* `file-name`: The name of the output file to retrieve

## Execution Deletion
**DELETE /api/system/processes/<:process-id>**

The will delete the process and associated files and output. Only processes with states of `SCHEDULED`, `COMPLETED` and `FAILED` can be deleted. 

Support for stopping a process can be included in a future version.

Status codes:
* 201 Created - if the operation succeed
* 404 Not found - if the processes doesn't exist
* 422 Unprocessable Entity - If the process is running


## Script Invocation

See the [scripts endpoint](scripts-endpoint.md#script-invocation) for details on how to start a script