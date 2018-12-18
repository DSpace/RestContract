# EPerson groups Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**GET /api/eperson/groups**

## Single EPerson Group
**GET /api/eperson/groups/<:uuid>**

```json
{
  "id": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
  "uuid": "617cf46b-535c-42d5-9d22-327ce2eff6dc",
  "name": "Administrator",
  "handle": null,
  "metadata": [
    {
      "key": "dc.title",
      "value": "Administrator",
      "language": null
    }
  ],
  "permanent": true,
  "type": "group",
  "_links": {
    "groups": {
      "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/groups"
    },
    "self": {
      "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc"
    }
  },
  "_embedded": {
    "groups": {
      "_embedded": {
        "groups": []
      },
      "_links": {
        "self": {
          "href": "https://dspace7-internal.atmire.com/rest/api/eperson/groups/617cf46b-535c-42d5-9d22-327ce2eff6dc/groups"
        }
      },
      "page": {
        "number": 0,
        "size": 0,
        "totalPages": 1,
        "totalElements": 0
      }
    }
  }
}
```

## Create new EPerson Group

**POST /api/eperson/groups**

To create a new EPerson Group, perform a post with the JSON below to the epersons endpoint when logged in as admin.

```json
{
  "name": "Administrator",
  "metadata": [
    {
      "key": "dc.title",
      "value": "Administrator"
    }
  ]
}
```
