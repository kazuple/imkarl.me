---
title: Signalling
description: 
published: true
date: 2020-08-18T18:39:45.468Z
tags: 
editor: markdown
---

# Overview
- The communication link between the client and server, or other clients that are used to control activities (for example, when a call is initiated), and deliver instant messages. 
- Most signaling traffic uses the **HTTPS-based REST** interfaces, though in some scenarios (**for example, connection between Microsoft 365 or Office 365 and a Session Border Controller**) it uses SIP protocol. 
- It's important to understand that this traffic is much less sensitive to latency but may cause service outages or call timeouts if latency between the endpoints exceeds several seconds.
- Utilises port 80 and 443.