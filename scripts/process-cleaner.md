# Process-Cleaner script
<!-- TOC -->
* [Process-Cleaner script](#process-cleaner-script)
  * [What it does?](#what-it-does)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The Process-Cleaner script is designed to clean up old processes in DSpace by removing processes in a specified state (
RUNNING, FAILED, or COMPLETED).

## When we use it?

This script is useful for cleaning up old processes that are no longer needed and taking up resources on the server.
Over time, these processes can accumulate, and the Process-Cleaner script can be used to remove them.

## How it Works?

The Process-Cleaner script queries the DSpace database to identify processes in the specified state (RUNNING, FAILED, or
COMPLETED). It then removes those processes from the database. By default, the Process-Cleaner script deletes all
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

In conclusion, the Process-Cleaner script is a useful tool for cleaning up old processes in DSpace. By removing old
processes, you can free up resources on the server and ensure that the system is running efficiently. To use the script,
specify the desired process state to remove. With this guide, you should now be able to use the Process-Cleaner script
effectively.
