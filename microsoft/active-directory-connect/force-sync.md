---
title: Force Sync
description: 
published: true
date: 2020-08-13T09:10:20.230Z
tags: 
editor: undefined
---

# Overview
Active Directory sncronisation tasks usally occur every 30 minutes when utilising a recent version of AADC.

In some cases you may want to force an AD sync, to initiate this follow the below steps.

# Step 1
Import the AD Sync module with the below command:

`Import-Module ADSync`

# Step 2
Run the following:

Delta Sync (most common, and used for most situations):

`Start-ADSyncSyncCycle -PolicyType Delta`

Full Sync (only necessary in some situations):

`Start-ADSyncSyncCycle -PolicyType Initial`
