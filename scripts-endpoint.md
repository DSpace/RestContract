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
      "description" : "add items to DSpace",
      "type" : "boolean"
    },
    {
      "name" : "-b",
      "description" : "add items to DSpace via Biblio-Transformation-Engine (BTE)",
      "type" : "boolean"
    },
    {
      "name" : "-c",
      "description" : "destination collection(s) database ID",
      "type" : "id"
    },
    {
      "name" : "-d",
      "description" : "delete items listed in mapfile",
      "type" : "file"
    },
    {
      "name" : "-e",
      "description" : "email of eperson doing importing",
      "type" : "string"
    },
    {
      "name" : "-i",
      "description" : "input type in case of BTE import",
      "type" : "string"
    },
    {
      "name" : "-n",
      "description" : "if sending submissions through the workflow, send notification emails",
      "type" : "boolean"
    },
    {
      "name" : "-p",
      "description" : "apply template",
      "type" : "string"
    },
    {
      "name" : "-q",
      "description" : "don't display metadata",
      "type" : "boolean"
    },
    {
      "name" : "-m",
      "description" : "mapfile items in mapfile",
      "type" : "file"
    },
    {
      "name" : "-r",
      "description" : "replace items in mapfile",
      "type" : "boolean"
    },
    {
      "name" : "-R",
      "description" : "resume a failed import (add only)",
      "type" : "boolean"
    },
    {
      "name" : "-t",
      "description" : "test run - do not actually import items",
      "type" : "boolean"
    },
    {
      "name" : "-w",
      "description" : "send submission through collection's workflow",
      "type" : "boolean"
    },
    {
      "name" : "-z",
      "description" : "zip file containing the import",
      "type" : "file"
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
