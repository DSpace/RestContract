# Workflow Definitions Endpoints
[Back to the list of all defined endpoints](endpoints.md)

A workflow definition is defined globally or per collection.  
It defines the list of steps to use in the workflow per collection, and the default steps.

All endpoints mentioned here require authentication, but no specific permissions.

## Main Endpoint
**/api/config/workflowdefinitions**   

Provide access to the configured workflow definitions. It returns the list of configured workflow-definitions.

## Single Workflow Definition
**/api/config/workflowdefinitions/<:definition-name>**

Provide detailed information about a specific workflow definition. The JSON response document is as follows:
```json
{
  "name": "default",
  "isDefault": true,
  "type": "workflow-definition"
}
```

Exposed links:
* collections: list of collections that explicitly use such workflow-definition
* steps: list of workflow steps included in this definition

### Search methods
#### findByCollection
**/api/config/workflowdefinitions/search/findByCollection?uuid=<:collection-uuid>**

It returns the workflow definition that applies to a specific collection eventually fallback to the default configuration.

### Linked entities
#### collections
**/api/config/workflowdefinitions/<:definition-name>/collections**

It returns the list of collections that make an explicit use of the workflow-definition.  
If a collection doesn't specify the workflow-definition to be used, the default mapping applies, but this collection is not included in the list returned by this method.

#### steps
**/api/config/workflowdefinitions/<:definition-name>/steps**

It returns the list of workflow-steps used in the workflow-definition. See [workflowsteps.md](workflowsteps.md).
