---
title: Direct Routing
description: 
published: true
date: 2020-09-04T08:21:21.046Z
tags: 
editor: markdown
---


Direct Routing allows you to use Microsoft Teams to manage inbound and outbound calls, just like a traditional PBX phone system.
# Base Configuration
## Pair SBC to Tenant
```
New-CsOnlinePSTNGateway -Fqdn <SBC FQDN> -SipSignalingPort <SBC SIP Port> -MaxConcurrentSessions <Max Concurrent Sessions the SBC can handle> -Enabled $true`
Get-CsOnlinePSTNGateway -Identity <SBC FQDN>
```

## Tenant Dial Plan
> **Only use if configuring a custom tenant dialplan!!**
{.is-info}
```
New-CsTenantDialPlan -Identity UK-TeamsDR-DP -Description "Dialplan for Teams Direct Routing UK normalization rules"
```

###  Normalisation Rules
> Normalization rules define how phone numbers expressed in various formats are to be translated. The same number string may be interpreted and translated differently, depending on the locale from which it is dialed. Normalization rules may be necessary if users need to be able to dial abbreviated internal or external numbers.

```
$NR = @()
$NR += New-CsVoiceNormalizationRule -Name "UK-London-Local" -Parent UK-TeamsDR-DP -Pattern '^([378]\d{7})$' -Translation '+4420$1' -InMemory -Description "Local number normalization for London"
$NR += New-CsVoiceNormalizationRule -Name 'UK-TollFree' -Parent UK-TeamsDR-DP -Pattern '^0((80(0\d{6,7}|8\d{7}|01111)|500\d{6}))\d*$' -Translation '+44$1' -InMemory -Description "UK Toll Free number normalisation"
$NR += New-CsVoiceNormalizationRule -Name 'UK-Premium' -Parent UK-TeamsDR-DP -Pattern '^0((9[018]\d|87[123]|70\d)\d{7})$' -Translation '+44$1' -InMemory -Description "UK Premium number normalization"
$NR += New-CsVoiceNormalizationRule -Name 'UK-Mobile' -Parent UK-TeamsDR-DP -Pattern '^0((7([1-57-9]\d{8}|624\d{6})))$' -Translation '+44$1' -InMemory -Description "UK Mobile number normalization"
$NR += New-CsVoiceNormalizationRule -Name 'UK-National' -Parent UK-TeamsDR-DP -Pattern '^0((1[1-9]\d{7,8}|2[03489]\d{8}|3[0347]\d{8}|5[56]\d{8}|8((4[2-5]|70)\d{7}|45464\d)))\d*(\D+\d+)?$' -Translation '+44$1' -InMemory -Description "UK National number normalization"
$NR += New-CsVoiceNormalizationRule -Name 'UK-Service' -Parent UK-TeamsDR-DP -Pattern '^(1(47\d|70\d|800\d|1[68]\d{3}|\d\d)|999|[\*\#][\*\#\d]*\#)$' -Translation '$1' -InMemory -Description "UK Service number normalization"
$NR += New-CsVoiceNormalizationRule -Name 'UK-International' -Parent UK-TeamsDR-DP -Pattern '^(?:\+|00)(1|7|2[07]|3[0-46]|39\d|4[013-9]|5[1-8]|6[0-6]|8[1246]|9[0-58]|2[1235689]\d|24[013-9]|242\d|3[578]\d|42\d|5[09]\d|6[789]\d|8[035789]\d|9[679]\d)(?:0)?(\d{6,14})(\D+\d+)?$' -Translation '+$1$2' -InMemory -Description "UK International number normalization"
```
Assign normalisation rules to dialplan
```
Set-CsTenantDialPlan -Identity UK-TeamsDR-DP -NormalizationRules @{add=$NR}
```
## PSTNUsage
> Container for Voice Routes and PSTN Usages; can be shared in different Voice Routing Policies
```
Set-CsOnlinePstnUsage -Identity Global -Usage @{Add="UK-Local","UK-Mobile","UK-TollFree","UK-Premium","UK-National","UK-International","UK-Service","UK-Withhold"}
```
## OnlineVoiceRoute
> Number pattern and set of Online PSTN Gateways to use for calls where calling number matches the pattern.
```
New-CsOnlineVoiceRoute -Name "UK-Local" -Priority 0 -OnlinePstnUsages "UK-Local" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+440?20([378]\d{7})' -Description "UK Local London routing"
New-CsOnlineVoiceRoute -Name "UK-Mobile" -Priority 1 -OnlinePstnUsages "UK-Mobile" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+44(7([1-57-9]\d{8}|624\d{6}))$' -Description "UK Mobile routing"
New-CsOnlineVoiceRoute -Name "UK-TollFree" -Priority 2 -OnlinePstnUsages "UK-TollFree" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+44(80(0\d{6,7}|8\d{7}|01111)|500\d{6})$' -Description "UK TollFree routing"
New-CsOnlineVoiceRoute -Name "UK-Premium" -Priority 3 -OnlinePstnUsages "UK-Premium" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+44(9[018]\d|87[123]|70\d)\d{7}$' -Description "UK Premium routing"
New-CsOnlineVoiceRoute -Name "UK-National" -Priority 4 -OnlinePstnUsages "UK-National" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+440?(1[1-9]\d{7,8}|2[03489]\d{8}|3[0347]\d{8}|5[56]\d{8}|8((4[2-5]|70)\d{7}|45464\d))' -Description "UK National routing"
New-CsOnlineVoiceRoute -Name "UK-International" -Priority 5 -OnlinePstnUsages "UK-International" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+((1[2-9]\d\d[2-9]\d{6})|((?!(44))([2-9]\d{6,14})))' -Description "UK International routing"
New-CsOnlineVoiceRoute -Name "UK-Service" -Priority 6 -OnlinePstnUsages "UK-Service" -OnlinePstnGatewayList <SBC FQDN> -NumberPattern '^\+?(1(47\d|70\d|800\d|1[68]\d{3}|\d\d)|999|[\*\#][\*\#\d]*\#)$' -Description "UK Service routing"
```

## OnlineVoiceRoutingPolicy
> Container for PSTN Usages; can be assigned to a user or to multiple users.
```
New-CsOnlineVoiceRoutingPolicy -Identity "UK-TeamsDR-VRP" -OnlinePstnUsages "UK-Local", "UK-Mobile", "UK-TollFree", "UK-Premium", "UK-National", "UK-International", "UK-Service", "UK-Withhold"
```


## Enable Users
> Assign DDI to user, enable enterprise voice and also Azure Voicemail.
```
Set-CsUser -ID "UPN" -OnPremLineURI tel:+44123456789 -EnterpriseVoiceEnabled $true -HostedVoiceMail $true
```
> Assign custom tenant dial plan to user.
```
Grant-CsTenantDialPlan -Identity "UPN" -PolicyName "UK-TeamsDR-DP"
```
> Assign OVRP to user.
```
Grant-CsOnlineVoiceRoutingPolicy -Identity "UPN" -PolicyName "UK-TeamsDR-VRP"
```
> Set user for Teams only mode (requirements for Direct Routing).
```
Grant-CSTeamsUpgradePolicy -Identity <User> -PolicyName <Policy>
```

---

# Routing Topologies
## Active, Active Outbound
### OnlineVoiceRoute
> Add a comma seperated list of SBC FQDN's to -OnlinePstnGatewayList for active active outbound calling.
```
New-CsOnlineVoiceRoute -Name "UK-Local" -Priority 0 -OnlinePstnUsages "UK-Local" -OnlinePstnGatewayList sbc01.imkarl.me,sbc02.imkarl.me -NumberPattern '^\+440?20([378]\d{7})' -Description "UK Local London routing"
```
## Non Media Bypass
![brk4014-callwithoutmediapass.png](/brk4014-callwithoutmediapass.png)
- PSTN HUB = Microsoft Azure DNS servers return an IP address pointing to the primary Azure datacenter assigned to the SBC. The assignment is based on performance metrics of the datacenters and geographical proximity to the SBC.
- MC = **Media Controller**. This is a microservice in Azure that assigns Media Processors and creates Session Description Protocol (SDP) offers.
- MP = **Media Processor**. This is a public facing component that handles media in non-bypass cases and handles media for voice applications.
- SIP Proxy = SIP Proxy is a component that translates HTTP REST signaling used in Teams to SIP.

## Local Media Optimisation
https://www.youtube.com/watch?v=liQg37lX7yg

# Misc
## Microsoft Root Certificate 
> The below certificate is required to communicate with Microsofts SIP Proxies via the Direct Routing enabled SBCs.
> https://cacert.omniroot.com/bc2025.pem
{.is-info}

