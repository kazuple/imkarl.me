---
title: SIP
description: 
published: true
date: 2020-08-03T20:12:22.727Z
tags: 
editor: undefined
---

# Methods
## INVITE
INVITE is used to initiate a session with a user agent. In other words, an INVITE method is used to establish a media session between the user agents.
### Example
> INVITE sips:Bob@TMC.com SIP/2.0 
>    Via: SIP/2.0/TLS client.ANC.com:5061;branch = z9hG4bK74bf9 
>    Max-Forwards: 70 
>    From: Alice<sips:Alice@TTP.com>;tag = 1234567 
>    To: Bob<sips:Bob@TMC.com>
>    Call-ID: 12345601@192.168.2.1  
>    CSeq: 1 INVITE 
>    Contact: <sips:Alice@client.ANC.com> 
>    Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY 
>    Supported: replaces 
>    Content-Type: application/sdp 
>    Content-Length: ...  
>    
>    v = 0 
>    o = Alice 2890844526 2890844526 IN IP4 client.ANC.com 
>    s = Session SDP 
>    c = IN IP4 client.ANC.com 
>    t = 3034423619 0 
>    m = audio 49170 RTP/AVP 0 
>    a = rtpmap:0 PCMU/8000
{.is-info}

## BYE

BYE is the method used to terminate an established session. This is a SIP request that can be sent by either the caller or the callee to end a session.

- It cannot be sent by a proxy server.
- BYE request normally routes end to end, bypassing the proxy server.
- BYE cannot be sent to a pending an INVITE or an unestablished session.

## REGISTER
REGISTER request performs the registration of a user agent. This request is sent by a user agent to a registrar server.

- The REGISTER request may be forwarded or proxied until it reaches an authoritative registrar of the specified domain.
- It carries the AOR (Address of Record) in the To header of the user that is being registered.
- REGISTER request contains the time period (3600sec).
- One user agent can send a REGISTER request on behalf of another user agent. This is known as third-party registration. Here, the From tag contains the URI of the party submitting the registration on behalf of the party identified in the To header.

Source: https://www.tutorialspoint.com/session_initiation_protocol/session_initiation_protocol_messaging.htm