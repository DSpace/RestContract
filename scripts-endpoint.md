# Admin Scripts Endpoints
[Back to the list of all defined endpoints](endpoints.md)

DSpace has functionality to import and export items in CSV and ZIP format, to start a collection harvest run, to run or schedule curation tasks, …. Each of these functionalities also map on a DSpace CLI script. While we could implement each of these operations as a separate endpoint, this contract tries to describe a generic endpoint that allows administrators to explore, run or schedule DSpace CLI scripts from the REST API.

## Scripts Endpoint
**GET /api/system/scripts**

This endpoint will list all (REST supported) scripts defined in `dspace/config/spring/rest/scripts.xml`. The script entries are embedded with a name, description and a self link. By "REST supported" we mean all scripts that have been updated to allow invocations from the REST API.

The JSON response document is as follows
```json
{
  "page": {
      	"size": 5,
      	"totalElements": 14,
      	"totalPages": 3,
      	"number": 0
  },
  "_embedded" : {
    "scripts" : [
      {
        "id" : "import",
        "name" : "import",
        "type" : "script",
        "description" : "Import items into DSpace",
        "_links" : {
          "self" : {
            "href" : "/api/system/scripts/import"
          }
        }
      },
      {
        "name" : "metadata-import",
        "type" : "script",
        "description" : "Import metadata after batch editing",
        "_links" : {
          "self" : {
            "href" : "/api/system/scripts/metadata-import"
          }
        }
      }
    ]
  }
}
```

## Script Name Endpoint

### Script Details
**GET /api/system/scripts/<:script-name>**

This endpoint will return information on all the parameters that are required to invoke the script. This depends on the parameters defined in the implementation of the script.

The JSON response document is as follows
```json
{
   "id" : "import",
   "name" : "import",
   "description" : "Import items into DSpace",
   "type" : "script",
   "parameters" : [
    {
      "name" : "-a",
      "nameLong" : "--add",
      "description" : "add items to DSpace",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-b", 
      "nameLong" : "--add-bte",
      "description" : "add items to DSpace via Biblio-Transformation-Engine (BTE)",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-c",      
      "nameLong" : "--collection",
      "description" : "destination collection(s) database ID",
      "type" : "id",
      "mandatory" : false
    },
    {
      "name" : "-d", 
      "nameLong" : "--delete",
      "description" : "delete items listed in mapfile",
      "type" : "file",
      "mandatory" : false
    },
    {
      "name" : "-e",
      "nameLong" : "--eperson",
      "description" : "email of eperson doing importing",
      "type" : "string",
      "mandatory" : false
    },
    {
      "name" : "-i",
      "nameLong" : "--inputtype",
      "description" : "input type in case of BTE import",
      "type" : "string",
      "mandatory" : false
    },
    {
      "name" : "-n",
      "nameLong" : "--notify",
      "description" : "if sending submissions through the workflow, send notification emails",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-p",
      "nameLong" : "--template",
      "description" : "apply template",
      "type" : "string",
      "mandatory" : false
    },
    {
      "name" : "-q",
      "nameLong" : "--quiet",
      "description" : "don't display metadata",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-m",    
      "nameLong" : "--mapfile",
      "description" : "mapfile items in mapfile",
      "type" : "file",
      "mandatory" : true
    },
    {
      "name" : "-r",   
      "nameLong" : "--replace",
      "description" : "replace items in mapfile",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-R",    
      "nameLong" : "--resume",
      "description" : "resume a failed import (add only)",
      "type" : "boolean"
    },
    {
      "name" : "-t",
      "nameLong" : "--test",
      "description" : "test run - do not actually import items",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-w",      
      "nameLong" : "--workflow",
      "description" : "send submission through collection's workflow",
      "type" : "boolean",
      "mandatory" : false
    },
    {
      "name" : "-z",
      "nameLong" : "--zip",
      "description" : "zip file containing the import",
      "type" : "file",
      "mandatory" : false
    }
   ]
}
```

Following parameter types are available:
* `string`: A regular string value
* `date`: A string that has a valid date pattern
* `boolean`: true or false
* `file`: For parameters with this type, the user has to provide a file. This would be a multipart POST request using the same filename
* `output`: Parameters with this type define the name of the output file. This name can be used later to download the the output file (e.g. when running `export` or `metadata-export`).

## Script Invocation
**POST /api/system/scripts/<:script-name>/processes**

POST requests to this endpoint will start the corresponding script with the provided parameters. All parameter values should be provided in the body that has to use the `multipart/form-data` content type. Once the upload is complete and the script was started successfully, this endpoint will return details on the scripts execution

Delayed scripts using a  `startTime` can be supported in a future version.

```json
{
  "processId" : "5",
  "userId" : "aa0263e2-b90a-4528-89fa-116ea4859de1",
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
      "href" : "/api/system/processes/5"
    },
    "script" : {
      "href" : "/api/system/scripts/import"
    },
    "output" : {
      "href" : "/api/system/processes/5/output"
    }
  }
}
```

The possible `status` values are `SCHEDULED`, `RUNNING`, `COMPLETED` and `FAILED`.

Status codes:
* 202 Accepted - if the task is accepted for processing
* 404 Not found - if the script doesn't exist
