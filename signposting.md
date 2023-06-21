# Signposting Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## The /signposting is used to implement Signposting.

Signposting is an approach to make the scholarly web more friendly to machines. It uses Typed Links as a means to clarify patterns that occur repeatedly in scholarly portals. For resources of any media type, these typed links are provided in HTTP Link headers. For HTML resources, they may additionally be provided in HTML link elements. More information at [Signposting the Scholarly Web](https://signposting.org/)

DSpace supports the FAIR signposting profile at level 2, see https://signposting.org/FAIR/

## Main Endpoint
```/signposting/linksets```

As we don't have yet an use case to iterate over all the existent linksets the main endpoint is not implemented and a 405 error code is returned according to our [general error response codes](README.md#error-codes).

## Single Item linkset

### in application/linkset+json serialization
```/signposting/linksets/<:uuid>/json```

This endpoint provides typed links of item having uuid specified in input and its resources, in application/linkset+json serialization

```json
{
  "linkset": [
    {
      "item": [
        {
          "href": "https://{dspace.ui.url}/bitstreams/{uuid}/download",
          "type": "text/plain"
        },
        {
          "href": "https://{dspace.ui.url}/bitstreams/{uuid}/download",
          "type": "application/pdf"
        }
      ],
      "type": [
        {
          "href": "https://schema.org/AboutPage"
        }
      ],
      "linkset": [
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}",
          "type": "application/linkset"
        },
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}/json",
          "type": "application/linkset+json"
        }
      ],
      "describedby": [
        {
          "href": "https://doi.org/10.1007/978-3-642-35233-1_18",
          "type": "application/vnd.datacite.datacite+json"
        },
        {
          "href": "https://{dspace.ui.url}/handle/123456789/17",
          "type": "text/html"
        }
      ],
      "cite-as": [
        {
          "href": "https://{dspace.ui.url}/handle/123456789/17"
        }
      ],
      "author": [
        {
          "href": "http://orcid.org/0000-0002-3748-8359",
          "type": "text/html"
        }
      ],
      "license": [
        {
          "href": "http://creativecommons.org/licenses/by/3.0/us/",
          "type": "text/html"
        }
      ],
      "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}",
    },
    {
      "collection": [
        {
          "href": "https://{dspace.ui.url}/entities/publication/{uuid}",
          "type": "text/html"
        }
      ],
      "linkset": [
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}",
          "type": "application/linkset"
        },
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}/json",
          "type": "application/linkset+json"
        }
      ],
      "anchor": "https://{dspace.ui.url}/bitstreams/{uuid}/download"
    },
    {
      "collection": [
        {
          "href": "https://{dspace.ui.url}/entities/publication/{uuid}",
          "type": "text/html"
        }
      ],
      "linkset": [
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}",
          "type": "application/linkset"
        },
        {
          "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}/json",
          "type": "application/linkset+json"
        }
      ],
      "anchor": "https://{dspace.ui.url}/bitstreams/{uuid}/download"
    },
    {
      "describes": [
        {
          "href": "https://{dspace.ui.url}/entities/publication/{uuid}",
          "type": "text/html"
        }
      ],
      "anchor": "https://{dspace.ui.url}/handle/123456789/17"
    },
    {
      "describes": [
        {
          "href": "https://{dspace.ui.url}/entities/publication/{uuid}",
          "type": "text/html"
        }
      ],
      "anchor": "https://doi.org/10.1007/978-3-642-35233-1_18"
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
<https://{dspace.ui.url}/bitstreams/{uuid}/download> ; rel="item" ; type="text/plain" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://{dspace.ui.url}/handle/123456789/29> ; rel="describedby" ; type="text/html" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://{dspace.ui.url}/handle/123456789/29> ; rel="cite-as" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" ,
<http://orcid.org/0000-0002-3748-8359> ; rel="author" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" ,
<https://isni.org/isni/0000002251201436> ; rel="author" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://{dspace.ui.url}/signposting/linksets/{uuid}> ; rel="linkset" ; type="application/linkset" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://{dspace.ui.url}/signposting/linksets/{uuid}/json> ; rel="linkset" ; type="application/linkset+json" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://schema.org/AboutPage> ; rel="type" ; anchor="https://{dspace.ui.url}/entities/publication/{uuid}" , 
<https://{dspace.ui.url}/signposting/linksets/{uuid}> ; rel="linkset" ; type="application/linkset" ; anchor="https://{dspace.ui.url}/bitstreams/{uuid}/download" , 
<https://{dspace.ui.url}/signposting/linksets/{uuid}/json> ; rel="linkset" ; type="application/linkset+json" ; anchor="https://{dspace.ui.url}/bitstreams/{uuid}/download" , 
<https://{dspace.ui.url}/entities/publication/{uuid}> ; rel="collection" ; type="text/html" ; anchor="https://{dspace.ui.url}/bitstreams/{uuid}/download" , 
<https://{dspace.ui.url}/entities/publication/{uuid}> ; rel="describes" ; type="text/html" ; anchor="https://{dspace.ui.url}/handle/123456789/29" , 
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
    "href": "http://orcid.org/orcidValue",
    "rel": "author",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://{dspace.ui.url}/bitstreams/{uuid}/download",
    "rel": "item",
    "type": "text/plain",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://doi.org/10.1007/978-3-642-35233-1_18",
    "rel": "describedby",
    "type": "application/vnd.datacite.datacite+json",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://{dspace.ui.url}/handle/123456789/40",
    "rel": "describedby",
    "type": "text/html",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://{dspace.ui.url}/handle/123456789/40",
    "rel": "cite-as",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}",
    "rel": "linkset",
    "type": "application/linkset",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://{dspace.ui.url}/signposting/linksets/{uuid}/json",
    "rel": "linkset",
    "type": "application/linkset+json",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  },
  {
    "href": "https://schema.org/AboutPage",
    "rel": "type",
    "anchor": "https://{dspace.ui.url}/entities/publication/{uuid}"
  }
]
```
Return codes:
* 200 OK - if the operation succeed
* 403 Forbidden - if you are not logged in with sufficient permissions for reading Item information.
* 404 Not found - if the item doesn't exist