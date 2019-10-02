# Submission-Forms Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A submission-form represents a single data entry form, associated with a submission section. It is configurable, and consists of one or more data entry fields. In DSpace 6 and below, this concept was called an "input-form" and was configured via input-forms.xml.

## Main Endpoint
**/api/config/submissionforms**   

Provide access to the configured submission-forms. It returns the list of existent submission-forms.
Please note that from DSpace 7 the submission-form concept has been introduced splitting the previous configuration file named input-forms.xml by page. Each submission-form describes a single page of inputs, combining different pages together is done with the [submission-definition](submissiondefinitions.md), aka the item-submission.xml configuration file 

Example: to be provided

## Single Submission-Form 
**/api/config/submissionforms/<:form-name>**

Provide detailed information about a specific input-form. The JSON response document is as follow
```json
{
  "id": "traditionalpageone",
  "name": "traditionalpageone",
  "rows": [
    {
      "fields": [
        {
          "input": {
            "type": "name"
          },
          "label": "Authors",
          "mandatory": false,
          "repeatable": true,
          "hints": "Enter the names of the authors of this item.",
          "selectableMetadata": [
            {
              "metadata": "dc.contributor.author",
              "label": null,
              "authority": null,
              "closed": null
            }
          ],
          "languageCodes": []
        }
      ]
    },
    {
      "fields": [
        {
          "input": {
            "type": "onebox"
          },
          "label": "Title",
          "mandatory": true,
          "repeatable": false,
          "mandatoryMessage": "You must enter a main title for this item.",
          "hints": "Enter the main title of the item.",
          "selectableMetadata": [
            {
              "metadata": "dc.title",
              "label": null,
              "authority": null,
              "closed": null
            }
          ],
          "languageCodes": []
        }
      ]
    },
    {
      "fields": [
        {
          "input": {
            "type": "onebox"
          },
          "label": "Identifiers",
          "mandatory": false,
          "repeatable": true,
          "hints": "If the item has any identification numbers or codes associated with\n                        it, please enter the types and the actual numbers or codes.",
          "selectableMetadata": [
            {
              "metadata": "dc.identifier.issn",
              "label": "ISSN",
              "authority": null,
              "closed": null
            },
            {
              "metadata": "dc.identifier.other",
              "label": "Other",
              "authority": null,
              "closed": null
            },
            {
              "metadata": "dc.identifier.ismn",
              "label": "ISMN",
              "authority": null,
              "closed": null
            },
            {
              "metadata": "dc.identifier.govdoc",
              "label": "Gov't Doc #",
              "authority": null,
              "closed": null
            },
            {
              "metadata": "dc.identifier.uri",
              "label": "URI",
              "authority": null,
              "closed": null
            },
            {
              "metadata": "dc.identifier.isbn",
              "label": "ISBN",
              "authority": null,
              "closed": null
            }
          ],
          "languageCodes": []
        }
      ]
    },
    {
      "fields": [
        {
          "input": {
            "type": "dropdown"
          },
          "label": "Type",
          "mandatory": false,
          "repeatable": true,
          "hints": "Select the type(s) of content of the item. To select more than one value in the list, you may\n                        have to hold down the \"CTRL\" or \"Shift\" key.",
          "selectableMetadata": [
            {
              "metadata": "dc.type",
              "label": null,
              "authority": "common_types",
              "closed": false
            }
          ],
          "languageCodes": []
        }
      ]
    },
    {
      "fields": [
        {
          "input": { },
          "label": "Journal",
          "mandatory": false,
          "repeatable": false,
          "hints": "Select the journal related to this volume.",
          "selectableRelationship": {
            "relationship": "isVolumeOfJournal",
            "filter": "creativework.publisher:somepublishername",
            "search-configuration": "periodicalConfiguration",
            "nameVariants": false
          }
        }
      ]
    },
    {
      "fields": [
        {
          "input": {
            "type": "name"
          },
          "label": "Author",
          "mandatory": true,
          "repeatable": true,
          "mandatoryMessage": "At least one author (plain text or relationship) is required",
          "hints": "Add an author",
          "selectableRelationship": {
            "relationship": "isAuthorOfPublication",
            "filter": null,
            "search-configuration": "personConfiguration",
            "nameVariants": true
          },
          "selectableMetadata": [
            {
              "metadata": "dc.contributor.author",
              "label": null,
              "authority": null,
              "closed": false
            }
          ],
          "languageCodes": []
        }
      ]
    }
  ],
  "type": "submissionform"
}

```

it is important to note that the field definition contains special attribute that in an ideal HAL representation should be replaced with links but for simplicity we have preferred to expose as string
* authority: the name of the authority used to retrieve value for the input [see authorities](authorities.md) 
* metadata: the key of the metadata field to use to store the input

The *scope* attribute can be null or one of the values: workflow or submission. A value other than null mean that the visibility of the field in the specified scope is defined by the value of the attribute *visibility.main* and *visibility.other* in the other scope. *Null* mean that the field will be visible in both the submission and workflow with the visibility specified in *visibility.main* 
The visibility attributes can assume one of the following values
* *null* : editable
* *readonly*: visible but not alterable
* *hidden*: not visible

The *nameVariants* attribute can only be used in combination with entities.
It defines whether an alternative name can be used for the stored relation with the entity (in the relationship).
