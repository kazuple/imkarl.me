---
title: SBC 5000 & 7000
description: 
published: true
date: 2020-12-18T17:43:13.677Z
tags: 
editor: markdown
dateCreated: 2020-08-18T19:21:04.235Z
---

# Terminology
- ERE - Embedded Routing Engine
- PSX - Policy and Routing
- ePSX - Embedded Policy and Routing
- EMA -  Embedded Management Application is a Web-based interface management system for configuring SBC servers. It supports configuration, fault, and security management functions.
- BMC - Baseboard Management Controller allows for system monitoring, power control, and configuring the Management Ports of the SBC. 

## IP Peer
- An IP Peer object is permanently bound to a Zone
- The primary purpose of this object is to facilitate
outbound call routing – an IP Peer reference is
returned by the policy server to indicate the IP address
or FQDN of the next hop for the call
- For inbound calls, the IP Peers associated with the
Zone are searched using the Peer’s IP address
- IP Peers are not used in access scenarios, where
individual peer devices are not known and where
outbound routing is done based on registrations rather
than routing to explicitly defined peers


## Address Context
- Container of objects that correspond to a specific IP Addressing domain. 
- Address contexts are used in the SBC to contain and segregate objects that deal with IP addresses.

## Zone
- Bound to an Address Context. Normally allocated by customer, carrier, organization, or service level.
 - Zone Objects Include
	− IP Peer(s) (Signaling Endpoints)
	− SIP or H323 Trunk Group(s)
	− SIP Signaling Port / H323 Signaling Port
