---
title: Policy
description: 
published: true
date: 2020-08-06T10:01:46.407Z
tags: 
editor: undefined
---

# Links
https://docs.microsoft.com/en-us/microsoftteams/assign-policies
https://docs.microsoft.com/en-us/powershell/module/teams/new-csgrouppolicyassignment?view=teams-ps
# New-CsGroupPolicyAssignment
> This is used to assign a policy to a security group or distribution list.
{.is-info}

- Group policy assignment allows you to easily manage policies across different subsets of users within your organization.
- Group policy assignment is recommended for groups of up to 50000 users, but it will also work with larger groups. 
- Group policy assignments are only propagated to users that are direct members of the group; the assignments are not propagated to members of nested groups.
## Policy Types
Group policy assignment is currently limited to the following policy types (as of  06/08/20):
- TeamsAppSetupPolicy
- TeamsCallingPolicy
- TeamsCallParkPolicy
- TeamsChannelsPolicy
- TeamsComplianceRecordingPolicy
- TeamsEducationAssignmentsAppPolicy
- TeamsMeetingBroadcastPolicy
- TeamsMeetingPolicy
- TeamsMessagingPolicy
## Example
`New-CsGroupPolicyAssignment -GroupId salesdepartment@contoso.com -PolicyType TeamsMeetingPolicy -PolicyName Kiosk`