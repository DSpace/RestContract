# subscription-send script
[Back to the script index](index.md)
<!-- TOC -->
* [subscription-send script](#subscription-send-script)
  * [What it does?](#what-it-does)
  * [Who can use it?](#who-can-use-it)
  * [When we use it?](#when-we-use-it)
  * [How it Works?](#how-it-works)
  * [Parameters](#parameters)
  * [Conclusion/QuickGuide:](#conclusionquickguide)
<!-- TOC -->
## What it does?

The subscription-send script is designed to send email notifications related to subscriptions in DSpace. Users can
subscribe to specific communities, collections, or items.

For more information,
see the [Email Subscription documentation](https://wiki.lyrasis.org/display/DSDOC7x/Email+Subscriptions)

## Who can use it?

This script can be used only by repository administrators.

## When we use it?

The subscription-send script is used to send periodic email updates based on the user's subscription frequency.

## How it Works?

The subscription-send script runs periodically (e.g., daily) on the DSpace server,
and sends email notifications to users
who have subscribed to specific communities, collections, or items.
The script queries the DSpace database to identify
users who have subscribed to specific items, and then generates an email notification for each subscription.

The script's frequency parameter specifies how often email notifications should be sent. For example, if the frequency
is set to "W" (week), email notifications will be sent once per week to users who have subscribed to items.

## Parameters

The subscription-send script has the following parameter:

| Parameter                        | Description                                                                         |
|----------------------------------|-------------------------------------------------------------------------------------|
| `-f`, <br/>`--frequency <value>` | The subscription frequency. Valid values include: D (day), W (week), and M (month). |

## Conclusion/QuickGuide:

In conclusion, the subscription-send script is a useful tool for sending email notifications to DSpace users who have
subscribed to specific items. By default, the script runs daily, but the frequency can be adjusted to match the needs of
your institution. If you have issues with users not receiving email notifications for their subscriptions, the
subscription-send script may be a helpful tool to diagnose and resolve the issue.
