# retry-tracker script
[Back to the script index](index.md)
<!-- TOC -->
* [retry-tracker script](#retry-tracker-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The retry-tracker script is used to retry failed commits to the OpenURLTracker in DSpace. The OpenURLTracker is used to
track requests made to external systems, such as DOI lookup services, and store metadata about those requests in the
DSpace database.

Also note
that this script is useful
in [exchanging usage statistics information with IRUS](https://wiki.lyrasis.org/display/DSDOC7x/Exchange+usage+statistics+with+IRUS).

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

When occasionally, these requests may fail due to network or service issues, and the retry-tracker script can be used to
retry those failed requests.

## How it Works?

The retry-tracker script queries the DSpace database to identify URLs that have been marked as failed in the
OpenURLTracker table. It then attempts to resend those failed requests. This can be useful in situations where requests
fail due to temporary network or service issues.

The `-a` option can be used to add a new URL to the OpenURLTracker table for testing purposes. This is useful for
testing the Retry-Tracker script without waiting for a real request to fail.

## Parameters

The following parameters are available for the Retry-Tracker script:

| Parameter           | Description                                                                                                                                   |
|---------------------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| `-a <value>`        | Add a new "failed" row to the table with a URL (test purposes only).                                                                          |
| `-r`                | Retry sending requests to all URLs stored in the table with failed requests. This includes the URL that can be added through the `-a` option. |
| `-h`, <br/>`--help` | Print the help message.                                                                                                                       |

## Conclusion/QuickGuide:

The retry-tracker script is a helpful tool for retrying failed requests to external systems in DSpace. It can help
ensure that important requests are not lost due to temporary network or service issues. If you experience frequent
issues with failed requests, the Retry-Tracker script may be a useful tool to add to your DSpace workflow.
