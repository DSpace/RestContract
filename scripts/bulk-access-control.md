# bulk-access-control script
[Back to the list of all defined scripts](./index.md)

<!-- TOC -->
* [bulk-access-control script](#bulk-access-control-management-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
* [What is an Access Condition?](#what-is-an-access-condition)
<!-- TOC -->
## What it does?

This script performs bulk changes to the access conditions of the items and bitstreams. 
The Bulk Access Control Management focus on Access Control rather than resource policies. The goal will be to add / replace access condition for a set of objects from a user friendly interface that will abstract from the complexity/internal rules of DSpace’s resource policies and bundles as much as possible.
It takes the list of object to process via the uuid parameter and the instructions about how to manipulate (replace, append) access conditions at the item and/or bitstream level. You can find more information about [what is an Access Condition?](#what-is-an-access-condition) at the end of the page.

## Who can use it?

This script can be used by any user that has administrative privileges over at least one community, collection or item limited to the resources that they admin.

## When we use it?

If you need to align the access conditions of all the items and/or bitstreams in a community or collection, or other selection to new rules defined by the Institution such as making open access all the thesis or restrict access to all the material related to a specific project, etc.


## How it Works?

The Bulk Access Control Management script reads a json file containing the details about which resources (items and bitstreams) need to be processed and how to manipulate the access conditions.
The script then processes the items and bitstreams identified according to the defined mode and specified accessConditions.

## Parameters

The script takes the following parameters:

| Parameter                        | Description                                                                                                                                                                            |
|----------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-u`, <br>`--uuid`               | This parameter, mandatory and repeatable, specifies the uuid of the object to process according to processing instruction, specified in the json file (see below).                                                                                                              |

| `-f`, <br>`--file`                | This parameter specifies the json file containing the processing instruction.                                                                                                              |
| `-h`, <br>`--help`               | This parameter displays help information for the script.                                                                                                                               |

The json file specified with the file parameter has the following structure
 structured as follow
```json
{
   "item": {
      "mode": "replace",
      "accessConditions": [
          {
            "name": "openaccess"
          }
      ]
   },
   "bitstream": {
      "constraints": {
          "uuid": ["bit-uuid1", "bit-uuid2", "...", "bit-uuidN"],
      },
      "mode": "add",
      "accessConditions": [
        {
         "name": "embargo",
         "startDate": "2024-06-24T23:59:59.999+0000"
        },
        {
         "name": "network"         
        }
      ]
   }
}
```
* item: the item node contains the processing detail at the item level for the identified resources
  * mode: can be replace or add. Replace will mean that after processing the items will have only the access conditions that are specified in the accessContitions property. Add will mean that after processing the items will have the specified accessCondition in addition to the ones already defined.
  * accessConditions: the list of access conditions to apply, with their specific parameters if needed (such as a startDate for an embargo, etc.). Only in replace mode the accessConditions can be empty in this case the access conditions of the item will be reset to what is inherited from their owningCollection. Please note that when accessConditions is set and is not empty not inheritence from the owningCollection occur. For more information on accessConditions see the [What is an Access Condition?](#what-is-an-access-condition) section below.
* bitstream: the `bitstream` node contains the processing details at the bitstream level for the identified resources. These changes will be applied to bitstreams in the main content bundle (i.e. ORIGINAL bundle) and to any corresponding, derivative bitstreams created by `filter-media`. The only exception is that any derivative bitstreams created with `publicPermission` (i.e. set to READ Anonymous at creation via `filter.org.dspace.app.mediafilter.publicPermission`) will keep their existing public policies.
  * constraints: only when the `--uuid` param (see above) contains just a single Item UUID, then the 'constraints' can be used to limit which Bitstreams are processed in that Item. If `constraints` specifies one or more bitstream UUIDs, then only those bitstreams listed will be processed. If the `constraint`s node is not present, is empty or contains a null uuid or 0-size list, then all the bitstreams will be processed.
  * mode: as per the item node
  * accessConditions: as per the item node

The following validations rules apply to the file
* item?.mode = replace|add
* bitstream?.mode = replace|add
* item or bitstream must be present
* item?.accessContitions[*].name & bitstream?.accessContitions[*].name must match a defined access conditions and the object must be validated against the access condition rules (max end date etc).
Failures to comply with these rules will lead to a failure in the script execution.

# What is an Access Condition?

An access condition is usually linked to a resource policy that grant READ permission to a specific eperson or group over a DSpace Object. It can be constrainst to a specific time windows with a start date or end date. READ permission are grant in DSpace to user for different purpose according to the different phase of an item life-cycle for this reason resource policy have a type, `TYPE_CUSTOM` or `TYPE_INHERITED` are used in policies that are set to define the final "access condition" of an object.

The access condition defined in the system are configured in a spring file of the the backend and exposed via the [Bulk Access Conditions endpoint](/bulkaccessconditionoptions.md)
