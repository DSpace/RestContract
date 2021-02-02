## Main Endpoint
**GET /api/integration/nbevents**

_Unsupported._ The suggestions can be retrieved only by source and target or via direct link, see the single entry and search method below. 

## GET Single suggestion
** GET /api/integration/nbevents/<:nbevent-id>

Return a single suggestion:

```json
{
  "id":"123e4567-e89b-12d3-a456-426614174000",
  "originalId": "oai:www.openstarts.units.it:10077/21486",
  "title":"Index nominum et rerum",
  "trust":"0.375",
  "eventDate": "2020/10/09 10:11 UTC",
  "status": "PENDING",
  "message" : {
    "type":"doi",
    "value":"10.18848/1447-9494/cgp/v15i09/45934",
    "abstracts":"Abstract Complex functionality of the ...",
    "acronym":"EPSILON",
    "code":"277749",
    "funder":"EC",
    "fundingProgram":"FP7",
    "jurisdiction":"EU",
    "title":"Elliptic Pdes and Symmetry of Interfaces and Layers for Odd Nonlinearities"
  },
  type: "nbevent"
}
```

Attributes
* the *id* attribute is the event primary key
* the *originalId* attribute is the oai identifier of the target publication
* the *title* attribute is the title of the publication as provided by openAIRE
* the *trust* attribute is the level of accuracy of the suggestion
* the *status* attribute is one of (ACCEPTED, REJECTED, DISCARDED, PENDING)
* the *eventDate* attribute is the timestamp of the event reception
* the *message* attribute is a json object which structure depends on the topic of the event. When the "topic" type is
    * ENRICH/MISSING/PID and ENRICH/MORE/PID: fills `message.type` with the type of persistent identifier (doi, pmid, etc.) and `message.value` with the corresponding value
    * ENRICH/MISSING/ABSTRACT: fills `message.abstract`
    * ENRICH/MISSING/SUBJECT/ACM: fills the `message.value` with the actual keywords, the subject classification is defined by the last part of the topic (ACM, JEL, DDC, etc.)
    * ENRICH/MISSING/PROJECT: fills `acronym`, `code`, `funder`, `fundingProgram`, `jurisdiction` and `title`

Exposed links:
* topic: link to the topic to which the event belong to
* target: link to the item that represent the targe to whom the suggestion apply
* related: link to a second item that is involved in the nb events (i.e. the project item)

Status codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no nb event exists with such id 

## Search methods
### Get nbevents by a given topic
**/api/integration/nbevents/search/findByTopic?topic=:target-key[&size=10&page=0]**

It returns the list of nb events from a specific topic

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* topic: mandatory, the key associated with the requested topic

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the topic parameter is missing or invalid

Provide paginated list of the nb events available.

## PATCH 
### To record a decision 
PATCH /api/integration/nbevents/<:nbevent-id>

This method allow users to Accept, Reject or Discard a nb event. The PATCH body must follow the JSON PATCH specification

```json
[
  {
    "op": "replace",
    "path": "/status",
    "value": "ACCEPTED|REJECTED|DISCARDED|PENDING"
  }
]
```

As response, the modified nb event will be returned.
 
## POST
### To associated a related item to the nb event
POST /api/integration/nbevents/<:nbevent-id>/related?item=<:item-uri>

Return codes:
* 201 Created - if the operation succeed
* 400 Bad Request - if the nb event doesn't allow a related item (i.e. it is not related to a */PROJECT topic)
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no nb event exists with such id

### To remove a related item to the nb event
DELETE /api/integration/nbevents/<:nbevent-id>/related

Only the association between the nb event and the related item id deleted. The related item stays untouched

Return codes:
* 204 No content - if the delete succeeded (including the case of no-op if the nb event didn't contain a related item)
* 400 Bad Request - if the nb event doesn't allow a related item (i.e. it is not related to a */PROJECT topic)
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no nb event exists with such id

### To replace a related item
Replacing a related item will require deleting the related association and creating a new association hereafter
