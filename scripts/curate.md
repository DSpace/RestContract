# curate script
[Back to the script index](index.md)
<!-- TOC -->
* [curate script](#curate-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->

## What it does?

The curate script allows you to select specific curation tasks to perform, and provides options for reporting and
queueing tasks.

For more information, see the [Curation System documentation](https://wiki.lyrasis.org/display/DSDOC7x/Curation+System)

## Who can use it?

This script can be used by any user that has administrative privileges over at least one community, collection or item limited to the resources that they admin.

## When we use it?

The curate script is used to perform curation tasks on items, collections, communities or the entire DSpace repository. Curation
tasks can be used to ensure that items meet certain quality standards, such as having required metadata or valid links.

## How it Works?

The curate script works by running curation tasks on items, collections, or the entire repository. Curation tasks are
specified using the **-t** or **-T** parameter, and can be configured using the **-p** parameter. The **-i** parameter
specifies the object to perform the task on, or "all" to perform the task on the entire repository. Curation tasks are
executed in a transaction scope, which can be specified using the **-s** parameter. The **-q** parameter can be used to
add tasks to a task queue for later processing, while the **-r** parameter can be used to generate a report on the
results of the curation task.

## Parameters

| Parameter                        | Description                                                                                                                   |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| `-t`, <br/>`--task <value>`      | The curation task name. Valid options include: `noop`, `profileformats`, `requiredmetadata`, `checklinks`, and `registerdoi`. |
| `-T`, <br/>`--taskfile <value>`  | The file containing curation task names.                                                                                      |
| `-i`, <br/>`--id <value>`        | The ID (handle) of the object to perform the task on, or `all` to perform on the entire repository.                           |
| `-p`, <br/>`--parameter <value>` | A task parameter in the format `NAME=VALUE`.                                                                                  |
| `-q`, <br/>`--queue <value>`     | The name of the task queue to process.                                                                                        |
| `-r`, <br/>`--reporter <value>`  | The relative or absolute path to the desired report file. Use `-` to report to console. If absent, no reporting.              |
| `-s`, <br/>`--scope <value>`     | The transaction scope to impose. Use `object`, `curation`, or `open`. If absent, `open` applies.                              |
| `-v`, <br/>`--verbose`           | Report activity to standard output.                                                                                           |
| `-h`, <br/>`--help`              | Display help information.                                                                                                     |

## Conclusion/QuickGuide:

The curate script is a powerful tool for performing various curation tasks on objects in DSpace. By specifying the
appropriate task name and parameters, users can perform a wide range of curation tasks on items in the repository. If
you're looking to automate curation tasks in DSpace, the curate script is definitely worth investigating.
