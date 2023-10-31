# Quality Assurance Events endpoint
The Quality Assurance feature aims to improve existing data in the repository using feedback that 
Quality Assurance sources (as OpenAIRE) made available, such as missing or additional metadata (abstract, subjects, PIDs, projects).

These endpoints provide access to the quality assurance events so that they can be reviewed and managed by the repository manager.

## GET All quality assurance events
**GET /api/integration/qualityassuranceevents**

_Unsupported._ The quality assurance events can be retrieved only by source and target or via direct link, see the single entry and search method below. 

## GET Single quality assurance event
**GET /api/integration/qualityassuranceevents/<:qualityassuranceevent-id>**

Return a single quality assurance event:

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
  "type": "qualityassuranceevent"
}
```

Attributes
* the *id* attribute is the event primary key
* the *originalId* attribute is the identifier used by the event's source for the target publication
* the *title* attribute is the title of the publication as provided by the correction's source
* the *trust* attribute is the level of accuracy of the quality assurance event (values from 0.00 to 1.00)
* the *status* attribute is one of (ACCEPTED, REJECTED, DISCARDED, PENDING)
* the *eventDate* attribute is the timestamp of the event reception
* the *message* attribute is a json object which structure depends on the source and on the topic of the event. When the source is "openaire" and the "topic" type is
    * ENRICH/MISSING/PID and ENRICH/MORE/PID: fills `message.type` with the type of persistent identifier (doi, pmid, etc.) and `message.value` with the corresponding value
    * ENRICH/MISSING/ABSTRACT: fills `message.abstract`
    * ENRICH/MISSING/SUBJECT/ACM: fills the `message.value` with the actual keywords, the subject classification is defined by the last part of the topic (ACM, JEL, DDC, etc.)
    * ENRICH/MISSING/PROJECT: fills `acronym`, `code`, `funder`, `fundingProgram`, `jurisdiction` and `title`

Exposed links:
* topic: link to the topic to which the event belong to (see [qualityassurancetopics](qualityassurancetopics.md))
* target: link to the item that represent the target to whom the quality assurance event apply
* related: link to an optional second item that is involved in the qa events (i.e. the project item for OpenAIRE ENRICH/MISSING/PROJECT event)

Status codes:
* 200 Ok - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no qa event exists with such id 

## Search methods
### Get qualityassuranceevents by a given topic
**GET /api/integration/qualityassuranceevents/search/findByTopic?topic=:topic-key[&size=10&page=0]**

It returns the list of qa events from a specific topic, eventually filtered by the target they refer to

The supported parameters are:
* page, size [see pagination](README.md#Pagination)
* topic: mandatory, the key associated with the requested topic. Please note that the topic could contain the uuid of a specific target item to restrict the qa events. See the note about the [qa topic id](qualityassurancetopics.md#get-single-topic)

Return codes:
* 200 OK - if the operation succeed
* 400 Bad Request - if the topic parameter is missing

Provide paginated list of the qa events available.

## PATCH 
### To record a decision 
PATCH /api/integration/qualityassuranceevents/<:qualityassuranceevent-id>

This method allow users to Accept, Reject or Discard a qa event. The PATCH body must follow the JSON PATCH specification

```json
[
  {
    "op": "replace",
    "path": "/status",
    "value": "ACCEPTED|REJECTED|DISCARDED|PENDING"
  }
]
```

As response, the modified qa event will be returned.
 
## POST
### To associated a related item to the qa event
**POST /api/integration/qualityassuranceevents/<:qualityassuranceevent-id>/related?item=<:item-uuid>**

This endpoint allows you to associate an associated item with the event, if the type of event supports it.

Return codes:
* 201 Created - if the operation succeed
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no qa event exists with such id
* 422 Unprocessable entity - if the qa event doesn't allow a related item (for example it is an openaire event not related to a */PROJECT topic)

### To remove a related item to the qa event
**DELETE /api/integration/qualityassuranceevents/<:qualityassuranceevent-id>/related**

Only the association between the qa event and the related item id deleted. The related item stays untouched

Return codes:
* 204 No content - if the delete succeeded (including the case of no-op if the qa event didn't contain a related item)
* 401 Unauthorized - if you are not authenticated
* 403 Forbidden - if you are not logged as an administrator
* 404 Not found - if no qa event exists with such id
* 422 Unprocessable entity - if the qa event doesn't allow a related item (i.e. it is not related to a */PROJECT topic)

### To replace a related item
Replacing a related item will require deleting the related association and creating a new association hereafter
