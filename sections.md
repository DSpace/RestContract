# Sections Endpoints
[Back to the list of all defined endpoints](endpoints.md)

## Main Endpoint

**/api/layout/sections**   

Provide access to all the configured dynamic layout sections. The full JSON response document is as follow
```json
{
  "_embedded" : {
    "sections" : [ {
      "id" : "researchoutputs",
      "componentRows" : [ [ {
        "browseNames" : [ "rodept", "author", "title", "type", "dateissued", "subject" ],
        "style" : "col-md-4",
        "componentType" : "browse"
      }, {
        "discoveryConfigurationName" : "researchoutputs",
        "style" : "col-md-8",
        "searchType" : null,
        "initialStatements" : 3,
        "displayTitle" : true,
        "componentType" : "search"
      } ], [ {
        "discoveryConfigurationName" : "researchoutputs",
        "sortField" : "dc.date.accessioned",
        "order" : "desc",
        "style" : "col-md-6",
        "titleKey" : null,
        "numberOfItems" : 5,
        "showThumbnails" : false,
        "componentType" : "top"
      }, {
        "discoveryConfigurationName" : "researchoutputs",
        "sortField" : "metric.view",
        "order" : "desc",
        "style" : "col-md-6",
        "titleKey" : null,
        "numberOfItems" : 5,
        "showThumbnails" : false,
        "componentType" : "top"
      } ], [ {
        "discoveryConfigurationName" : "researchoutputs",
        "style" : "col-md-12",
        "facetsPerRow" : 4,
        "componentType" : "facet"
      } ] ],
      "nestedSections" : [ ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/researchoutputs"
        }
      }
    }, {
      "id" : "fundings_and_projects",
      "componentRows" : [ ],
      "nestedSections" : [ {
        "id" : "fundings",
        "componentRows" : [ [ {
          "browseNames" : [ "pjtitle" ],
          "style" : "col-md-4",
          "componentType" : "browse"
        }, {
          "discoveryConfigurationName" : "project_funding",
          "style" : "col-md-8",
          "searchType" : null,
          "initialStatements" : 3,
          "displayTitle" : true,
          "componentType" : "search"
        } ], [ {
          "discoveryConfigurationName" : "project_funding",
          "style" : "col-md-12",
          "facetsPerRow" : 4,
          "componentType" : "facet"
        } ] ],
        "nestedSections" : [ ],
        "type" : "section",
        "uniqueType": "layout.section"
      }, {
        "id" : "projects",
        "componentRows" : [ [ {
          "browseNames" : [ "pjtitle" ],
          "style" : "col-md-4",
          "componentType" : "browse"
        }, {
          "discoveryConfigurationName" : "project_funding",
          "style" : "col-md-8",
          "searchType" : null,
          "initialStatements" : 3,
          "displayTitle" : true,
          "componentType" : "search"
        } ], [ {
          "discoveryConfigurationName" : "project_funding",
          "style" : "col-md-12",
          "facetsPerRow" : 4,
          "componentType" : "facet"
        } ] ],
        "nestedSections" : [ ],
        "type" : "section",
        "uniqueType": "layout.section"
      }, {
        "id" : "researcherprofiles",
        "componentRows" : [ [ {
          "browseNames" : [ "rpname", "rpdept" ],
          "style" : "col-md-4",
          "componentType" : "browse"
        }, {
          "discoveryConfigurationName" : "person",
          "style" : "col-md-8",
          "searchType" : null,
          "initialStatements" : 3,
          "displayTitle" : true,
          "componentType" : "search"
        } ], [ {
          "discoveryConfigurationName" : "person",
          "style" : "col-md-12",
          "facetsPerRow" : 4,
          "componentType" : "facet"
        } ] ],
        "nestedSections" : [ ],
        "type" : "section",
        "uniqueType": "layout.section"
      }, {
        "id" : "site",
        "componentRows" : [ [ {
          "style" : "style",
          "content" : "cms.homepage.header",
          "contentType" : "text-metadata",
          "componentType" : "text-row"
        } ], [ {
          "discoveryConfigurationName" : "site",
          "style" : "col-md-12",
          "searchType" : "basic",
          "initialStatements" : 3,
          "displayTitle" : false,
          "componentType" : "search"
        } ], [ {
          "style" : "col-md-12 py-4",
          "counterSettingsList" : [ {
            "discoveryConfigurationName" : "researchoutputs",
            "icon" : "fas fa-file-alt fa-3x",
            "entityName" : "publications",
            "link" : "/explore/researchoutputs"
          }, {
            "discoveryConfigurationName" : "project_funding",
            "icon" : "fas fa-cogs fa-3x",
            "entityName" : "project_funding",
            "link" : "/explore/fundings_and_projects"
          }, {
            "discoveryConfigurationName" : "person",
            "icon" : "fas fa-users fa-3x",
            "entityName" : "rprofiles",
            "link" : "/explore/researcherprofiles"
          } ],
          "componentType" : "counters"
        } ], [ {
          "discoveryConfigurationName" : "homePageTopItems",
          "sortField" : "dc.date.accessioned",
          "order" : "desc",
          "style" : "col-md-6",
          "titleKey" : null,
          "numberOfItems" : 5,
          "showThumbnails" : false,
          "componentType" : "top"
        }, {
          "discoveryConfigurationName" : "homePageTopItems",
          "sortField" : "metric.view",
          "order" : "desc",
          "style" : "col-md-6",
          "titleKey" : null,
          "numberOfItems" : 5,
          "showThumbnails" : false,
          "componentType" : "top"
        } ] ],
        "nestedSections" : [ ],
        "type" : "section",
        "uniqueType": "layout.section"
      } ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/fundings_and_projects"
        }
      }
    }, {
      "id" : "researcherprofiles",
      "componentRows" : [ [ {
        "browseNames" : [ "rpname", "rpdept" ],
        "style" : "col-md-4",
        "componentType" : "browse"
      }, {
        "discoveryConfigurationName" : "person",
        "style" : "col-md-8",
        "searchType" : null,
        "initialStatements" : 3,
        "displayTitle" : true,
        "componentType" : "search"
      } ], [ {
        "discoveryConfigurationName" : "person",
        "style" : "col-md-12",
        "facetsPerRow" : 4,
        "componentType" : "facet"
      } ] ],
      "nestedSections" : [ ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/researcherprofiles"
        }
      }
    }, {
      "id" : "site",
      "componentRows" : [ [ {
        "style" : "style",
        "content" : "cms.homepage.header",
        "contentType" : "text-metadata",
        "componentType" : "text-row"
      } ], [ {
        "discoveryConfigurationName" : "site",
        "style" : "col-md-12",
        "searchType" : "basic",
        "initialStatements" : 3,
        "displayTitle" : false,
        "componentType" : "search"
      } ], [ {
        "style" : "col-md-12 py-4",
        "counterSettingsList" : [ {
          "discoveryConfigurationName" : "researchoutputs",
          "icon" : "fas fa-file-alt fa-3x",
          "entityName" : "publications",
          "link" : "/explore/researchoutputs"
        }, {
          "discoveryConfigurationName" : "project_funding",
          "icon" : "fas fa-cogs fa-3x",
          "entityName" : "project_funding",
          "link" : "/explore/fundings_and_projects"
        }, {
          "discoveryConfigurationName" : "person",
          "icon" : "fas fa-users fa-3x",
          "entityName" : "rprofiles",
          "link" : "/explore/researcherprofiles"
        } ],
        "componentType" : "counters"
      } ], [ {
        "discoveryConfigurationName" : "homePageTopItems",
        "sortField" : "dc.date.accessioned",
        "order" : "desc",
        "style" : "col-md-6",
        "titleKey" : null,
        "numberOfItems" : 5,
        "showThumbnails" : false,
        "componentType" : "top"
      }, {
        "discoveryConfigurationName" : "homePageTopItems",
        "sortField" : "metric.view",
        "order" : "desc",
        "style" : "col-md-6",
        "titleKey" : null,
        "numberOfItems" : 5,
        "showThumbnails" : false,
        "componentType" : "top"
      } ] ],
      "nestedSections" : [ ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/site"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/layout/sections"
    },
    "search" : {
      "href" : "http://localhost/api/layout/sections/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 4,
    "totalPages" : 1,
    "number" : 0
  }
}
```


## Search Endpoint

**/api/layout/sections/search/visibleTopBarSections**   

Lists all the dynamic layout sections for the top bar marked as visible.
The full JSON response document is as follow
```json
{
  "_embedded" : {
    "sections" : [ {
      "id" : "sectionresearchoutputs",
      "componentRows" : [ [ {
        "discoveryConfigurationName" : null,
        "style" : null,
        "searchType" : null,
        "initialStatements" : 3,
        "displayTitle" : true,
        "componentType" : "search"
      } ] ],
      "nestedSections" : [ ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/sectionresearchoutputs"
        }
      }
    }, {
      "id" : "sectionfundings_and_projects",
      "componentRows" : [ [ {
        "discoveryConfigurationName" : null,
        "style" : null,
        "searchType" : null,
        "initialStatements" : 3,
        "displayTitle" : true,
        "componentType" : "search"
      } ] ],
      "nestedSections" : [ ],
      "type" : "section",
      "uniqueType": "layout.section",
      "_links" : {
        "self" : {
          "href" : "http://localhost/api/layout/sections/sectionfundings_and_projects"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/layout/sections/search/visibleTopBarSections"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 2,
    "totalPages" : 1,
    "number" : 0
  }
}
```
Return codes:
* 200 OK - The operation succeed

* `sections`: is an array, it contains the sections to be shown in the nav bar
* `size` - the dimension of the result set window returned (can be different from the requested value due to imposed limit, see the request parameters section)
* `totalElements` - the total size of the result set
* `totalPages` - the number of available page of result using the current window size
* `number` - the index (zero-based) of the returned page

## Single Section
**/api/layout/sections/<:id>**

Provide detailed information about a specific dynamic layout section. The JSON response document is as follow
```json
{
  "id" : "publications",
  "componentRows" : [ [ {
    "browseNames" : [ "rodept", "author", "title", "type", "dateissued", "subject" ],
    "style" : "col-md-4",
    "componentType" : "browse"
  }, {
    "discoveryConfigurationName" : "publication",
    "style" : "col-md-8",
    "searchType" : null,
    "initialStatements" : 3,
    "displayTitle" : true,
    "componentType" : "search"
  } ], [ {
    "discoveryConfigurationName" : "publication",
    "sortField" : "dc.date.accessioned",
    "order" : "desc",
    "style" : "col-md-6",
    "titleKey" : null,
    "numberOfItems" : 5,
    "showThumbnails" : false,
    "componentType" : "top"
  }, {
    "discoveryConfigurationName" : "publication",
    "sortField" : "metric.view",
    "order" : "desc",
    "style" : "col-md-6",
    "titleKey" : null,
    "numberOfItems" : 5,
    "showThumbnails" : false,
    "componentType" : "top"
  } ], [ {
    "discoveryConfigurationName" : "publication",
    "style" : "col-md-12",
    "facetsPerRow" : 4,
    "componentType" : "facet"
  } ] ],
  "nestedSections" : [ ],
  "type" : "section",
  "uniqueType": "layout.section",
  "_links" : {
    "self" : {
      "href" : "http://localhost/api/layout/sections/publications"
    }
  }
}
```
Return codes:
* 200 OK - if the operation succeed
* 404 Not found - if a section with the given id doesn't exist

The `componentRows` attribute represent the list of components that compose the section, splitted by rows. There are 4 different types of components, distinguishable by their `componentType` attribute which can have one of the following values: [ `browse`,`top`,`facet`,`search`,`text-row`, ,`counters` ]. The attributes of each component may vary according to the type of the component itself. In addition to the `componentType` attribute used to indicate the type of the component, each component type has the following attributes:
* `browse`: has only one attribute named `browseNames` representing a list of names
* `top`: has the `discoveryConfigurationName` indicating the discovery configuration name and two attributes named `sortField` and `order` indicating by which field and in what order to sort the search results, `numberOfItems` indicating how many items will have to be displayed in the section , `style` the style to set, `titleKey` and `showThumbnails` (this last not yet used by angular component)
* `facet`: has the `discoveryConfigurationName` indicating the discovery configuration name,  and the optional `facetsPerRow` defining how many facet box UI will display.
* `search`: has the `discoveryConfigurationName` indicating the discovery configuration name. searchType with value `basic` or `advanced` (default) defining whether displayed box should consist in a single input box or many containing multiple statements that can be combined with AND, OR, NOT keywords.
* `text-row`: has the `order` property, defining the order on which the content should appear among other text row elements in the same list of dynamic layout elements,  `content` property, defining the static content to be displayed, while `contentType` defines the type of content, its value can be `image`, meaning that content is an image url, `text-raw` meaning that content is a static text to be displayed and `text-key` meaning actual text to be displayed should be rendered using UI’s I18n logic
* `counters` : has `counterSettingsList` property, each element representing a counter to be displayed by this infographic and having following properties: `discoveryConfigurationName`, the discovery configuration to be used to count by query elements; `icon`, a font awesome reference to icon to be displayed (i.e. `fas fa-book fa-3x`), label text key to be displayed together with the icon, `link`(optional) link to be followed when icon is clicked.
