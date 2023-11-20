# process-cleaner script
[Back to the script index](index.md)
<!-- TOC -->
* [process-cleaner script](#process-cleaner-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The process-cleaner script is designed to clean up old processes in DSpace by removing processes in a specified state
(RUNNING, FAILED, or COMPLETED).

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

This script is useful for cleaning up old processes that are no longer needed and taking up resources on the server.
Over time, these processes can accumulate, and the process-cleaner script can be used to remove them.

## How it Works?

The process-cleaner script queries the DSpace database to identify processes in the specified state (RUNNING, FAILED, or
COMPLETED). It then removes those processes from the database. By default, the process-cleaner script deletes all
processes in the COMPLETED state. The `-r` and `-f` options can be used to delete processes in the RUNNING and FAILED
states, respectively.

## Parameters

This script has the following parameters:

| Parameter                  | Description                                                                      |
|----------------------------|----------------------------------------------------------------------------------|
| - `-h`, <br/>`--help`      | Print the help message.                                                          |
| - `-r`, <br/>`--running`   | Delete the process with RUNNING status.                                          |
| - `-f`, <br/>`--failed`    | Delete the process with FAILED status.                                           |
| - `-c`, <br/>`--completed` | Delete the process with COMPLETED status (default if no statuses are specified). |

## Conclusion/QuickGuide:

In conclusion, the process-cleaner script is a useful tool for cleaning up old processes in DSpace. By removing old
processes, you can free up resources on the server and ensure that the system is running efficiently. To use the script,
specify the desired process state to remove.
