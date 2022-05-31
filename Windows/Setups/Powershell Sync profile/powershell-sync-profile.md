---
title: Powershell Sync profile
parent: Setups
grand_parent: Windows
tags: windows, powershell
---

# Sync Powershell profile with Github
This will describe how I am using Github to sync and update my powershell profile (note however that this is probably not best practice since I will be loading scripts from the internet. Do not just copy and paste)


This script will create/overwrite the existing three profiles for PS7, PS5 and ISE

```powershell
# Create profile for both PS Core, PS and ISE
$PS7path = "C:\Users\$ENV:USERNAME\Documents\PowerShell\Microsoft.PowerShell_profile.ps1"
$PS5path = "C:\Users\$ENV:USERNAME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
$PS5ISEpath = "C:\Users\$ENV:USERNAME\Documents\WindowsPowerShell\Microsoft.PowerShellISE_profile.ps1"


New-Item -Path $PS7path -Force 
New-Item -Path $PS5path -Force
New-Item -Path $PS5ISEpath -Force
```

# Add

```powershell
$content = @"
# Load profile from Github
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/qhrizz/Public/master/Windows/Powershell/Profile.ps1'))
"@

Set-Content -Path $PS7path -Value $content
Set-Content -Path $PS5path -Value $content
Set-Content -Path $PS5ISEpath -Value $content
```


The one thing you should change is line 15, where fetch and load the powershell script from Github. This can probably be any URL where the file is hosted in "clear text"

Now everything you put in that profile will be loaded. 

My "prod" file can be found here https://raw.githubusercontent.com/qhrizz/Public/master/Windows/Powershell/Profile.ps1

