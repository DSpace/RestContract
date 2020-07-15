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
              "controlledVocabulary": null,
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
              "controlledVocabulary": null,
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
              "controlledVocabulary": "common_types",
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
              "controlledVocabulary": null,
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

The json has an ordered array named rows containing the fields that need to be rendered. The structure of each field is as follow
* input: contains the details about the widget `type` to use (onebox, dropdown, date, etc.) and if applicable a `regex` to use to validate user input.  Some input types provide specific behaviors:
    * `dropdown` type: requires use of a (non-hierarchical) controlled vocabulary. Provides a selectbox/dropdown of entries within that vocabulary.
    * `lookup` type: requires use of a controlled vocabulary. Provides searching capabilities across that vocabulary to select a specific entry or entries.
    * `lookup-name` type: Same as `lookup`, but specific to names.
    * `onebox` type: acts as a single text input field. Would provide auto-suggest capabilities when associated with a (non-hierarchical) controlled vocabulary.  If the associated controlled vocabulary is hierarchical (`hieararchical=true`), then the onebox input type provides access to the hierarchical selection tree for that vocabulary.
* scope: the *scope* attribute can be null or one of the values: workflow or submission. A value other than null mean that the visibility of the field in the specified scope is defined by the value of the attribute *visibility.main* and *visibility.other* in the other scope. *Null* mean that the field will be visible in both the submission and workflow with the visibility specified in *visibility.main* 
* visibility: the visibility attributes can assume one of the following values
    * `null` : editable
    * `readonly`: visible but not alterable
    * `hidden`: not visible
* label: the label to present to the user for this field 
* mandatory: *true* if the user must provides a value for this field
* repeatable: *true* if the field allows multiple values
* mandatoryMessage: the error message to present to the user if no value if supplied for this field
* hints: contextual help to present to the user for this field 
* style: css class or classes to apply to the html componenents related to this field. They should be added as is to the html block containing the field, it can be for example col-md-3 to constraint the field in the bootstrap grid
* selectableMetadata: most of time one field is associated exactly with one metadata but it is possible, using the qualdrop_value in the submission-forms.xml, associate one field with a list of different qualified metadata. In such case the selectableMetadata will contains an actual array of available metadata for the field otherwise a single entry will be present. Each entry in the selecatebleMetadata array will be as follow
    * metadata: the key of the metadata field to use to store the input
    * controlledVocabulary: the name of the controlled vocabulary used to retrieve value for the input [see controlled vocabularies](vocabularies.md)
    * label: an optional label to associate with the selector of the metadata
* selectableRelationship: it is similar to the selectableMetadata but used when the field use relations underhood
    * relationshipType: the name of the relationship
    * filter: a filter to apply searching for candidate related items
    * searchConfiguration: the name of the discovery configuration to use to search for candidate related items
    * nameVariants: It defines whether an alternative name can be used for the stored relation with the entity (in the relationship)
* languageCodes contains the array of languages that can be assigned to an user input. For each language it is present a *display* string (i.e. English, Italian, etc.) and a *code* (i.e. en, it).
  
It is important to note that the field definition contains special attributes in selectableMetadata and selectableRelationship that in an ideal HAL representation should be replaced with links but for simplicity we have preferred to expose as string.