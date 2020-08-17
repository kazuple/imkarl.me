---
title: Decomission
description: 
published: true
date: 2020-08-13T13:17:12.627Z
tags: 
editor: undefined
---

# Step 1 - Migrate Users to Microsoft Teams
Migrate users directly from Skype for Business Server to Teams utilising the -MoveToTeams property on the Move-CsUser cmdlet. This will directly migrate the users to Teams and set (on a per user basis) their co-existance mode to TeamsOnly. 

Run the following command to migrate a user:
`Move-CsUser -Identity UPN -Target sipfed.online.lync.com -UseOAuth -MoveToTeams`

> Run the above command via Skype for Business Management Shell or PowerShell via the use of the "SkypeForBusiness" module which is packaged with the Skype for Business Server administrative tools.
{.is-info}

# Step 2 - Change DNS Records
The following records will need to be added/modified/removed.

## External Records
Modify the following:
| Record                 | Type  | Port | TTL  | Destination            |
|------------------------|-------|------|------|------------------------|
| sip                    | CNAME |      |      | sipdir.online.lync.com |
| lyncdiscover           | CNAME |      |      | webdir.online.lync.com |
| _sipfederationtls._tcp | SRV   | 5061 | 3600 | sipfed.online.lync.com |
| _sip._tls.             | SRV   | 443  | 3600 | sipdir.online.lync.com |

Remove the following (example):
| Record       | Type |
|--------------|------|
| dial-in      | A    |
| meet         | A    |
| lync-web     | A    |
| owa          | A    |
| _xmpp-server | SRV  |

## Internal Records
> Remove any DNS records which are related to your decomissioned Skype for Business deployment.
{.is-info}

# Step 3 - Disable Shared SIP Address Space
Login to Skype for Business Online PowerShell and run the following command:
`Set-CsTenantFederationConfiguration –SharedSipAddressSpace $false`

# Step 4 - Disable SfB Users
Remove users by running the following command:
`Get-CsUser | Disable-CsUser -Verbose`
> This will disable every user through the whole Skype for Business topology.
{.is-warning}

> Once a user is disabled the msRTCSIP attributes will be removed from the user. If this is not the case visit [here](https://msunified.net/2019/07/11/my-post-migration-from-skype-to-teams-toolbox/) to find a script to clear the attributes.
{.is-info}

# Step 5 - Disable Skype for Business Federation
Remove federation configuration and Skype for Business Online hosting provider from the on-premises deployment.
`Set-CsAccessEdgeConfiguration –AllowOutsideUsers $false –AllowFederatedUsers $false`

`Remove-CsHostingProvider –Identity "HOSTING-PROVIDER-NAME"`

# Step 6 - Monitor users InterpretedUserType
Now that the users have been disabled and the msRTCSIP attributes are cleared we can monitor the migrated users InterpretedUserType to change to "DirSyncSfBUser". 

> If msRTCSIP-DeploymentLocator still has "sipfed.online.lync.com" configured the command can be used to clear this configuration. This will keep the users in a "HybridOnlineSfBUser" InterpretedUserType if the attribute is not cleared.
{.is-warning}

> `Get-ADUser -Filter {UserPrincipalName -eq "UPN"} -Properties * | Set-ADUser -clear msRTCSIP-DeploymentLocator`
{.is-warning}

---

From here we are able to focus on the decomission process for your on-premises deployment. Yours users are now utilising Teams directly without the needs for the hybrid topology.

---

# Step 7 - Remove Skype for Business deployment
1. Connect to a Front End server in your topology and open the Topology Builder.
2. Click Action and navigate to Topology.
3. Click Remove Deployment.
4. Progress through the "Remove Deployment" wizard.
5. We will then need to remove any existing Conferencing Directories via the below command:
`Get-CsConferenceDirectory | Remove-CsConferenceDirectory`
> -Force property may be required.
{.is-info}
6. Remove any Voice Routes which are currently utilised via a PSTN gateway integration with the below command:
`Get-CsVoiceRoute | Remove-CsVoiceRoute`
7. Remove PSTN Gateway references in the Topology Builder.
8. Click Action > Topology > Publish. This will publish the removal configuration confirmed in the previous step.
9. Progress through the "Publish the topology" wizard.
10. Remove any Exchange UM Contacts with the following command:
`Get-CsExUmContact | Remove-CsExUmContact`
11. Remove any Call Park Orbit policies with the following command:
`Get-CsCallParkOrbit | Remove-CsCallParkOrbit`
12. Remove any Unassigned Number configuration with the following command:
`Get-CsUnassignedNumber | Remove-CsUnassignedNumber`
13. To finalize the Skype for Business topology decomission we would run the below command:
`Publish-CsTopology -FinalizeUninstall`
14. Remove the Configuration Store Location from Active Directory via the following command:
`Remove-CsConfigurationStoreLocation `

# Step 8 - Remove Skype for Business Active Directory preperation configuration
The following commands will remove the additional schema configuration which was added when the original Skype for Business topology was deployed.

`Disable-CsAdDomain -Domain contoso.com -GlobalSettingsDomainController DC.contoso.com -Force`
> Disable-CsAdDomain command will remove the RTC groups from your AD permissions. 
{.is-info}

`Disable-CsAdForest -Force -GroupDomain contoso.com`
> Disable-CsAdForest will remove the CS groups from your AD. 
{.is-info}

> Make sure the account you are utilising is in the "Enterprise Administrators" group or you will get permission issues. 
{.is-info}

# Step 9 - Remove Firewall Rules
Now that the Skype for Business enviroment has been decomissioned this gives you the chance to remove any existing firewall rules which were in use for enabling external functionality via the edge server.