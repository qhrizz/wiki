---
title: Cheat sheet
parent: Stuff
grand_parent: M365
tags: m365
---

# Cheat sheet
## List all mailboxes with a forwardingaddress set
```Powershell
Get-Mailbox | Where-Object {$_.ForwardingSMTPAddress -ne $null} | Select-Object DisplayName,PrimarySMTPAddress,ForwardingSMTPAddress,Whencreated
```

## Get MFA status
```Powershell
Get-MsolUser -all | Select-Object DisplayName,UserPrincipalName,@{N="MFA Status"; E={ if( $_.StrongAuthenticationRequirements.State -ne $null){ $_.StrongAuthenticationRequirements.State} else { "Disabled"}}}
```

## Teamsgroup export with membercount, owners, whencreated etc
```Powershell
Write-Output "DisplayName;Address;Owner;Members;WhenCreated;WhenChanged" | Out-File C:\temp\groups.csv
$groups = Get-UnifiedGroup 
foreach($group in $groups)
    {
        $owner = (Get-UnifiedGroupLinks "$($group.DisplayName)" -LinkType Owner).Name -join "|"
        $memberCount = (Get-UnifiedGroupLinks "$($group.DisplayName)" -LinkType member).count
        "$($group.DisplayName)" + ";" + "$($group.PrimarySMTPAddress)" + ";" + $owner + ";" + $memberCount + ";" + "$($group.WhenCreated)" + ";" + "$($group.WhenChanged)" | out-file C:\temp\groups.csv -Append
    }
```

## List all users and the assigned plans including when it was assigned. Export to CSV
```
Get-AzureADUser | Select-Object displayName -ExpandProperty assignedplans | Export-Csv C:\slask\test2.csv -NoTypeInformation -Encoding UTF8
```

Or you can match one serviceplan from https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/licensing-service-plan-reference

```Powershell
Get-AzureADUser | Where-Object {$_.AssignedPlans.ServicePlanId -match "d5156635-0704-4f66-8803-93258f8b2678"} | Select-Object displayName -ExpandProperty assignedplans | Where-Object {$_.Serviceplanid -match "d5156635-0704-4f66-8803-93258f8b2678"} | Export-Csv C:\slask\test.csv -NoTypeInformationCheck when licenses were assigned (actually when the service was assigned)
```

## List available Account SKUIDs (licenses)
```Powershell
Get-MsolAccountSku 
```

## List all services enabled for a user with a specific license

```Powershell
((Get-MsolUser -UserPrincipalName "user@domain.com").Licenses | Where-Object {$_.AccountSkuId -match "<AccountSku>"}).ServiceStatus
```

## Re-enable services for an account (all)
```Powershell
$licenseObject = New-MsolLicenseOptions -AccountSkuId "<AccountSku>" 
Set-MsolUserLicense -UserPrincipalName "user@domain.com" -LicenseOptions $licenseObject 
```