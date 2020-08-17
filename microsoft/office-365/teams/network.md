---
title: Network
description: 
published: true
date: 2020-08-06T10:25:42.796Z
tags: 
editor: undefined
---

# Ports
- Clients can connect directly on any high port (1,024-65,535 UDP) to the services in Office 365 if there is nothing blocking them.
-- The traffic via high ports will not go via the Transport Relay (that's where we use 3478-3481 UDP)
# Bandwidth Requirements
- 30 kbps 	Peer-to-peer audio calling
- 130 kbps 	Peer-to-peer audio calling and screen sharing
- 500 kbps 	Peer-to-peer quality video calling 360p at 30fps
- 1.2 Mbps 	Peer-to-peer HD quality video calling with resolution of HD 720p at 30fps
- 1.5 Mbps 	Peer-to-peer HD quality video calling with resolution of HD 1080p at 30fps
- 500kbps/1Mbps 	Group Video calling
- 1Mbps/2Mbps 	HD Group video calling (540p videos on 1080p screen)

Source: https://docs.microsoft.com/en-us/microsoftteams/prepare-network#bandwidth-requirements