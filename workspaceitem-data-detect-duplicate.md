# WorkspaceItem data of detect-duplicate sectionType
[Back to the definition of the workspaceitems endpoint](workspaceitems.md)

This section data represents the metadata collected for a specific [submissionsection](submissionsections.md).
It is a json object with the following structure:

```json
{
	"detect-duplicate": {
		"matches": {
			"a3d46767-8cbd-4cea-915f-c3f055ffab01": {
				"submitterDecision": null,
				"workflowDecision": null,
				"adminDecision": null,
				"submitterNote": null,
				"workflowNote": null,
				"matchObject": {
					"id": "a3d46767-8cbd-4cea-915f-c3f055ffab01",
					"uuid": "a3d46767-8cbd-4cea-915f-c3f055ffab01",
					"name": "Test Item",
					"handle": "123456789/3",
					"metadata": {
						"dc.contributor.author": [
							{
								"value": "Shepherd, K",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.date.accessioned": [
							{
								"value": "2022-07-06T21:24:37Z",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.date.available": [
							{
								"value": "2022-07-06T21:24:37Z",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.date.issued": [
							{
								"value": "2022",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.description.provenance": [
							{
								"value": "Made available in DSpace on 2022-07-06T21:24:37Z (GMT). No. of bitstreams: 1\nf.txt: 447 bytes, checksum: 607b02f0036b93c1d0a03eaaa48c37e9 (MD5)\n  Previous issue date: 2022",
								"language": "en",
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.identifier.uri": [
							{
								"value": "http://localhost:4000/handle/123456789/3",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dc.title": [
							{
								"value": "Test Item",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"dspace.entity.type": [
							{
								"value": "Publication",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						],
						"gi.identifier.uri": [
							{
								"value": "https://www.example-uri.com",
								"language": null,
								"authority": null,
								"confidence": -1,
								"place": 0
							}
						]
					},
					"inArchive": true,
					"discoverable": true,
					"withdrawn": false,
					"lastModified": "2022-07-06T22:19:28.664+00:00",
					"entityType": "Publication",
					"type": "item"
				}
			}
		}
	}
}
```

If the section is empty, no matches were found.

## Patch operations
The PATCH method expects a JSON body according to the [JSON Patch specification RFC6902](https://tools.ietf.org/html/rfc6902)


