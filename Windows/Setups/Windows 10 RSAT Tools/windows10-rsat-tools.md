---
title: Install RSAT Tools in Windows 10
parent: Setups
grand_parent: Windows
tags: windows, rsat
---

# Install RSAT Tools with Powershell on Windows 10

1. Check which capabilities you want 
```
Get-WindowsCapability -Name RSAT* -Online | Select-Object name
```

2. Install 

```
Add-WindowsCapability -Name "Rsat.Dns.Tools~~~~0.0.1.0" -Online
```

3. Done

## Example
DHCP, ADUC, DNS and GPO

```
Add-WindowsCapability -Name "Rsat.Dns.Tools~~~~0.0.1.0" -Online
Add-WindowsCapability -Name "Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0" -Online
Add-WindowsCapability -Name "Rsat.DHCP.Tools~~~~0.0.1.0" -Online
Add-WindowsCapability -Name "Rsat.GroupPolicy.Management.Tools~~~~0.0.1.0" -Online

```