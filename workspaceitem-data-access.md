# WorkspaceItem data of access sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represent the data about the access condition.
This section will have the same name as the configuration available off the /api/config/submissionaccessoptions/. As an example, a WorkspaceItem section here named itemAccessConditions would correspond to a configuration /api/config/submissionaccessoptions/itemAccessConditions.

```json
{
  "discoverable": true,
  "accessConditions": [
    {
      "id": 123,
      "name": "openaccess"
    },
    {
      "id": 126,
      "name": "administrator"
    },
    {
      "id": 127,
      "name": "embargo",
      "startDate": "2018-06-24T00:40:54.970+0000"
    },
    {
      "id": 128,
      "name": "lease",
      "endDate": "2017-12-24T00:40:54.970+0000"
    }
  ]
}
```
* discoverable: if the current item not discoverable, this meaning that It's simply hidden from all search/browse/OAI results, and is therefore only accessible via direct link (or bookmark). To manage the access restriction (aka resource policies) see the accessCondition property below.
* accessConditions: an array of all the policies that has been applied by the user to the item. 

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body. See also the [General rules for the Patch operation](patch.md) for more details.

### Add Access Condition
To add an access condition the client must send a JSON Patch ADD operation as follow

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/accessConditions/-", "value": {name: "...", endDate: ".."}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to **replace completely** the access conditions applied to a specific workspaceItem

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/accessConditions", "value": [{name: "...", endDate: ".."}]}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

The exact attributes that the access condition to add must have depend on the name attribute that will identify the access condition configuration as exposed by the [submissionaccessoptions configuration endpoint](submissionaccessoptions.md).
For instance the following request is valid

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/accessConditions/-", "value": {name: "openaccess"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

instead the request

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/accessConditions/-", "value": {name: "embargo"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

will be rejected because a startDate attribute is also expected when an embargo is applied (according to the example configuration listed in this doc for the [submissionaccessoptions configuration endpoint](submissionaccessoptions.md). 

The right request will be

`curl --data '{[ { "op": "add", "path": "/sections/<:name-of-the-form>/accessConditions/-", "value": {name: "embargo", startDate: "2018-12-31"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

Please note that doesn't make sense to add an access condition in a specific index and, to simplify the client implementation it was assumed that the accessCondition array is never *null* but eventually an empty array so that you call always append new AccessCondition to the path "/sections/access /accessCondition/-".

### Remove Access Condition
To remove a previous applied access condition it is sufficient to invoke a remove operation on the accessCondition position path, i.e.

`curl --data '{[ { "op": "remove", "path": "/sections/<:name-of-the-form>/accessConditions/<:access-idx>"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

the following request will reset the access condition to the empty array

`curl --data '{[ { "op": "remove", "path": "/sections/<:name-of-the-form>/accessConditions"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`

### Replace
The replace operation allows to replace *existent* information with new one. Attempt to use the replace operation without a previous value must return an error. See [general errors on PATCH requests](patch.md)

#### Discoverable
To replace a previous discoverable flag value it is sufficient to invoke a replace operation on the discoverable position path, i.e.

`curl --data '{[ { "op": "replace", "path": "/sections/<:name-of-the-form>/discoverable", "value": true|false }]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

For example, starting with the following eperson field data:
```json
 "discoverable": true,
```
the replace operation `[{ "op": "replace", "path": "/sections/<:name-of-the-form>/discoverable", "value": false]` will result in :
```json
  "discoverable": false,
```
 For access configurations that do not allow the user to specify the visibility of the item, attempts to change the discoverable flag result in a response with 422 status from the server.

#### Access Condition
You can replace an existent access condition with a new one or update some settings of an existent access condition using the replace operation. The new settings must be valid for the selected access condition (name) according to the [submissionaccessoptions configuration endpoint](submissionaccessoptions.md).

To replace an access condition with a new one

`curl --data '{[ { "op": "replace", "path": "/sections/<:name-of-the-form>/accessConditions/0", "value": {name: "embargo", startDate: "2018-12-31"}}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to update the embargo start date

`curl --data '{[ { "op": "replace", "path": "/sections/<:name-of-the-form>/accessConditions/0/startDate", "value": "2019-12-31"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

to transform an existent *openaccess* access condition to an *administrator* access condition

`curl --data '{[ { "op": "replace", "path": "/sections/<:name-of-the-form>/accessConditions/0/name", "value": "administrator"}]}' -X PATCH ${dspace7-url}/api/submission/workspaceitems/<:id>`

please note that the above works only because of openaccess and administrator have the same settings needs (no need of addition information). Indeed, the backend is expected to remove the existent policy and create a new policy. If the settings of the previous and new access condition differs the request must fail with a 422 error code

## Relationship between the access conditions of the item and the bitstreams
Access conditions set on an item can be overwritten by specific access conditions set directly on the bitstreams via [workspaceitem data upload](workspaceitem-data-upload.md). If a bistream has no specific access conditions then it inherits those of the item; if an access conditions is inplace for the bitstream, then those of the item are ignored and are not applied to it.
