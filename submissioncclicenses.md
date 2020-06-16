# Submission CC Licenses Endpoints
[Back to the list of all defined endpoints](endpoints.md)

The proposed structure is derived from the configuration options already available in DSpace 6 and below.
In the first implementation, a single configuration named *default* is expected as these configurations were set at the site level.
Introducing an endpoint to manage a collection of configurations we open the door to future extension where different setups can be used for different kind of submissions (theses, technical reports, journal articles, etc.)

## Main Endpoint
**/api/config/submissioncclicenses**   

Provide access to the potential licenses for the CC license. It returns the list of configured CC licenses.

This will correspond to e.g. https://api.creativecommons.org/rest/1.5/ filtered to only include the licenses which are configured for this repository

```json
{
  "_embedded": {
    "submissioncclicenses": [
      {
        "id": "publicdomain",
        "name": "Public Domain",
        "type": "submissioncclicenses",
        "fields": []
      },
      {
        "id": "mark",
        "name": "Public Domain Mark",
        "type": "submissioncclicenses",
        "fields": []
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

## Single CC License 
**/api/config/submissioncclicenses/<:license-name>**

Provide detailed information about a specific license. Some licenses are basic, e.g. zero retrieved from https://api.creativecommons.org/rest/1.5/license/zero
```json
{
  "id": "publicdomain",
  "name": "CC0",
  "type": "submissioncclicenses",
  "fields": [],
  "_links" : {
    "self" : {
      "href" : "/api/config/submissioncclicenses/publicdomain"
    }
  }
}
```

Some licenses contain questions, e.g. standard retrieved from https://api.creativecommons.org/rest/1.5/license/standard
```json
{
  "id": "standard",
  "name": "CC0",
  "type": "submissioncclicenses",
  "fields": [
    {
      "id": "commercial",
      "label": "Allow commercial uses of your work?",
      "description": "The licensor permits others to copy, distribute and transmit the work. In return, licensees may not use the work for commercial purposes — unless they get the licensor's permission.",
      "values": [
        {
          "id": "y",
          "label": "Yes",
          "description": "The licensor permits others to copy, distribute, display, and perform the work, including for commercial purposes."
        },
        {
          "id": "n",
          "label": "No",
          "description": "The licensor permits others to copy, distribute, display, and perform the work for non-commercial purposes only."
        }
      ]
    },
    {
      "id": "derivatives",
      "label": "Allow modifications of your work?",
      "description": "The licensor permits others to copy, distribute and transmit only unaltered copies of the work — not derivative works based on it.",
      "values": [
        {
          "id": "y",
          "label": "Yes",
          "description": "The licensor permits others to copy, distribute, display and perform the work, as well as make derivative works based on it."
        },
        {
          "id": "sa",
          "label": "ShareAlike",
          "description": "The licensor permits others to distribute derivative works only under the same license or one compatible with the one that governs the licensor's work."
        },
        {
          "id": "n",
          "label": "No",
          "description": "The licensor permits others to copy, distribute and transmit only unaltered copies of the work — not derivative works based on it."
        }
      ]
    }
  ],
  "_links" : {
    "self" : {
      "href" : "/api/config/submissioncclicenses/standard"
    }
  }
}
```

The fields are questions to be answered when assigning the license

## Search CC License 
**/api/config/submissioncclicenseurls/search/rightsByQuestions**

This endpoint is used to obtain the final CC License URI after (optionally) providing answers for the given license. 
The URI may differ depending on the answers that are provided, and answers are only required if the given license requires them. 
To use this endpoint, you must first request information about a specific license using the `/api/config/submissioncclicenses/<:license-name>` endpoint above.

Parameters:
* license: the ID of the license. This identifier must be a license returned from the `/api/config/submissioncclicenses` endpoint (e.g. standard, publicdomain, …)
* answer_X: List of answers. X must be a valid field ID returned from `/api/config/submissioncclicenses/<:license-name>`, and the answer value must be a valid value ID for that field.  (e.g. answer_commercial=y&answer_derivatives=sa)

If the combination of the license and the answers is valid, it will return the license URI.

```json
{
  "url": "http://creativecommons.org/licenses/by-sa/3.0/us/",
  "type": "submissioncclicenseUrl",
  "_links": {
    "self": {
      "href": "https://dspace7-internal.atmire.com/server/api/config/submissioncclicenses/search/rightsByQuestions?license=standard&answer_commercial=y&answer_derivatives=sa"
    }
  }
}
```

Response codes:
* 200 OK - if the operation succeeded
* 400 Bad Request - If the answers provided aren't valid or if a required answer is missing  
* 401 Unauthorized - if you are not logged in
* 404 Not found - if the provided license isn't valid
