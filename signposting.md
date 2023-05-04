# Signposting Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## The /signposting is used to implement Signposting.

Signposting is an approach to make the scholarly web more friendly to machines. It uses Typed Links as a means to clarify patterns that occur repeatedly in scholarly portals. For resources of any media type, these typed links are provided in HTTP Link headers. For HTML resources, they may additionally be provided in HTML link elements. More information at [Signposting the Scholarly Web](https://signposting.org/) 

DSpace supports the FAIR signposting profile at level 2, see https://signposting.org/FAIR/

## Main Endpoint
```/signposting/linksets```

As we don't have yet an use case to iterate over all the existent linksets the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#Error codes).

## Single Item linkset 

### in application/linkset+json serialization
```/signposting/linksets/<:uuid>/json```

This endpoint provides typed links of item having uuid specified in input, in application/linkset+json serialization

```json
{
  "linkset": [
    {
      "item": [
        {
          "href": "https://{dspace.url}/bitstream/123456789/7/1/test.txt",
          "type": "text/plain"
        },
        {
          "href": "https://{dspace.url}/bitstream/123456789/7/1/test.pdf",
          "type": "application/pdf"
        }
      ],
      "author": [
        {
          "href": "http://orcid.org/0000-0002-3748-8359"
        },
        {
          "href": "https://isni.org/isni/0000002251201436"
        }
      ],
      "cite-as": [
        {
          "href": "https://doi.org/10.1007/978-3-642-35233-1_18"
        }
      ],
      "anchor": "https://{dspace.url}/handle/123456789/7"
    },
    {
      "anchor": "https://{dspace.url}/bitstream/123456789/7/1/test.txt",
      "collection": [
        {
          "href": "https://{dspace.url}/handle/123456789/7",
          "type": "text/html"
        }
      ]
    },
    {
      "anchor": "https://{dspace.url}/bitstream/123456789/7/1/test.pdf",
      "collection": [
        {
          "href": "https://{dspace.url}/handle/123456789/7",
          "type": "text/html"
        }
      ]
    }
  ]
}
```
Return codes:
* 200 OK - if the operation succeed
* 403 Forbidden - if you are not logged in with sufficient permissions for reading Item information.
* 404 Not found - if the item doesn't exist

### in application/linkset serialization
```/signposting/linksets/<:uuid>```

This endpoint provides typed links of item having uuid specified in input, in application/linkset serialization

```
<https://{dspace.url}/bitstream/123456789/7/1/test.txt> ; rel="item" ; type="text/plain" ; anchor="https://{dspace.url}/handle/123456789/7" ,
<https://{dspace.url}/bitstream/123456789/7/1/test.pdf> ; rel="item" ; type="application/pdf" ; anchor="https://{dspace.url}/handle/123456789/7" , 
<http://orcid.org/0000-0002-3748-8359> ; rel="author" ; anchor="https://{dspace.url}/handle/123456789/7" ,
<https://isni.org/isni/0000002251201436> ; rel="author" ; anchor="https://{dspace.url}/handle/123456789/7" , 
<https://doi.org/10.1007/978-3-642-35233-1_18> ; rel="cite-as" ; anchor="https://{dspace.url}/handle/123456789/7" ,
<https://{dspace.url}/handle/123456789/7> ; rel="collection" ; type="text/html" anchor="https://{dspace.url}/bitstream/123456789/7/1/test.txt" ,
<https://{dspace.url}/handle/123456789/7> ; rel="collection" ; type="text/html" anchor="https://{dspace.url}/bitstream/123456789/7/1/test.pdf" ,
```
Return codes:
* 200 OK - if the operation succeed
* 403 Forbidden - if you are not logged in with sufficient permissions for reading Item information.
* 404 Not found - if the item doesn't exist

### linkset data for response header or meta tag
```/signposting/links/<:uuid>```

This endpoint provides typed links, of item or bitstream having uuid specified in input, to be included item or bitstream page response header or meta tag

```json
[
  {
    "href": "https://{dspace-url}/linksets/{uuid}/lset",
    "rel": "linkset",
    "type": "application/linkset"
  },
  {
    "href": "https://{dspace-url}/linksets/{uuid}/json",
    "rel": "linkset",
    "type": "application/linkset+json"
  },
  {
    "href": "https://{dspace-url}/handle/123456789/1",
    "rel": "collection",
    "type": "text/html"
  }
]
```
Return codes:
* 200 OK - if the operation succeed
* 403 Forbidden - if you are not logged in with sufficient permissions for reading Item information.
* 404 Not found - if the item doesn't exist