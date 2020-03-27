# WorkspaceItem data of CC License sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

The section data represents the data about the CC license

```json
{
  	"uri": "http://creativecommons.org/licenses/by-nc-sa/3.0/us/",
  	"rights": "Attribution-NonCommercial-ShareAlike 3.0 United States",
	"file":  
  	 	{
          "uuid": "1ce6db0e-662f-4a13-ba87-c371ad664b14",
          "name": "license_rdf",
          "handle": null,
          "metadata": {
              "dc.title": [
                {
                  "value": "license_rdf",
                  "language": null,
                  "authority": null,
                  "confidence": -1,
                  "place": 0
                }
              ]
          },
          "sizeBytes": 1012,
          "checkSum": {
            "checkSumAlgorithm": "MD5",
            "value": "59a4cfd819ce67ba7252efc52d1eb99b"
          },
          "sequenceId": 1,
          "type": "bitstream"
        }
}
```

The following attributes can be defined:
* uri: the URI of the CC License, typically stored in dc.rights.uri, null is no license is granted
* rights: the rights name of the CC License, typically stored in dc.rights, null is no license is granted
* file: the bitstream containing the license, null is no license is granted

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)

Each successful Patch operation will return a HTTP 200 CODE with the new workspaceitem as body

### Add
To accept a CC license the client must send a JSON Patch ADD operation of the uri retrieved from the [Search CC License](submissioncclicenses.md#search-cc-license)

```json
[
  {
    "op": "add",
    "path": "/sections/<:name-of-the-form>/uri",
    "value": "http://creativecommons.org/licenses/by-nc-sa/3.0/us/"
  }
]
```

The dc.rights, dc.rights.uri and RDF bitstream will be retrieved from e.g. https://api.creativecommons.org/rest/1.5/details?license-uri=http://creativecommons.org/licenses/by-nc-sa/3.0/ 

Please note that according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902) a subsequent add operation on the `/sections/<:name-of-the-form>/uri` will have the effect to replace the previous granted license with a new one. 
In this case a new CC license will be added to the item and the previous license deleted.

This use of the add operation to replace the license could be counter intuitive but it is done according to the [RFC section 4.1](https://tools.ietf.org/html/rfc6902#section-4.1)
> If the target location specifies an object member that does exist, that member's value is replaced.

### Remove
It is possible to remove a previously granted CC license 
`curl --data '{[ { "op": "remove", "path": "/sections/<:name-of-the-form>/uri"}]' -X PATCH ${dspace7-url}/api/submission/workspaceitems/1`
