# Sitemaps Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint
**/api/discover/sitemaps**   

_Unsupported._ As sitemaps don't have a JSON representation, there's no use in this endpoint

## Single Sitemap
**/api/discover/sitemaps/<:name>**

This will return the contents of a given sitemap, typically as HTML or XML. Sample URLs
* /api/discover/sitemaps/htmlmap contains a list of links to other maps (links to REST) with content type HTML
* /api/discover/sitemaps/htmlmap_0 contains a list of links to items (links to Angular) with content type HTML
* /api/discover/sitemaps/xmlmap contains an XML with links to other maps (links to REST) with content type XML
* /api/discover/sitemaps/xmlmap_0 contains an XML with links to items (links to Angular) with content type XML

Response Headers:
* **Content-Type**: the mimetype of the sitemap 
* **Content-Length**: the size in bytes of the sitemap
* **Last-Modified**: when the sitemap was computed

The supported **Request Headers** are:
* If-Modified-Since: only return the content if the file is modified more recently

Response codes:
* 200 OK - if the sitemap exists
* 404 Not found - if the sitemap doesn't exist
