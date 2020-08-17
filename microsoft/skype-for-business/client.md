---
title: Client
description: 
published: true
date: 2020-07-21T21:07:47.052Z
tags: 
editor: undefined
---

# Logging
The Skype for Business client writes the following types of errors to the Windows ETL files, along with detailed troubleshooting information:

- Errors that prevent a user from logging on to the server, such as host or domain name errors or a valid certificate that is not valid
- Diagnostic messages returned by the server, such as version check failures, problems with log-in credentials, or errors generated in response to a SIP INVITE message from the client

## Tracing file directory

> Lync 2010 client: C:/Users/#username#/Tracing
{.is-info}

> Lync 2013 client: C:/Users/#username#/AppData/Local/Microsoft/Office/15.0/Lync/Tracing
{.is-info}

> Skype for Business client: C:/Users/#username#/AppData/Local/Microsoft/Office/15.0/Lync/Tracing
{.is-info}

## Enable logging on Skype for Business client

Start the Skype Client > Options > General > Logging > select the Full logging in Skype for business and Turn on Windows Event logging check box

![032618_1256_skypeforbus1.png](/032618_1256_skypeforbus1.png)

After that you should restart the client and then try to reproduce the issue
After the issue has appeared, exit the Lync client
Locate the tracing files in the directory referenced above.

Source: http://msexchangeguru.com/2018/03/26/business-client-logging/