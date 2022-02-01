---
title: Powershell cheat sheet and nice stuff to have
parent: Stuff
grand_parent: Windows
tags: windows, powershell
---


# Powershell nice to have
A collection of nice to have things.

## Check last reboot time
```
Get-CimInstance -ClassName win32_operatingsystem | Select-Object csname, lastbootuptime
```

## Check windows update
```
(New-Object -com "Microsoft.Update.AutoUpdate"). Results | Format-List
```

## Scheduled Computer restart
Call the function. DateTime in format "2021-02-10 12:00" 
Example: 
```
New-RebootTask -dateTime "2021-02-10 12:00"
```
```
function New-RebootTask{
    param(
        [string]$dateTime 
    )
    if($null -eq (Get-ScheduledTask -TaskName "Reboot computer once")){
        $action = New-ScheduledTaskAction -Execute 'Powershell.exe' ` -Argument '-NoProfile -WindowStyle Hidden -command "& {Restart-Computer -Force}"' 
        $trigger = New-ScheduledTaskTrigger -Once -At "$dateTime" 
        $principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest 
        Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "Reboot computer once" -Description "Reboot computer once" -Principal $principal
    }
    else{
        Get-ScheduledTask -TaskName "Reboot computer once" | Unregister-ScheduledTask -Confirm:$false
        $action = New-ScheduledTaskAction -Execute 'Powershell.exe' ` -Argument '-NoProfile -WindowStyle Hidden -command "& {Restart-Computer -Force}"' 
        $trigger = New-ScheduledTaskTrigger -Once -At "$dateTime" 
        $principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest 
        Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "Reboot computer once" -Description "Reboot computer once" -Principal $principal
    }
}

``` 

**Examples** 
Restart once, date and timeformat should be yyyy-dd-mm hh:mm
`$trigger = New-ScheduledTaskTrigger -Once -At "2018-11-29 03:00"`

Restart every week 
`$trigger = New-ScheduledTaskTrigger -Weekly -WeeksInterval 1 -DaysOfWeek Friday -At 3am`

## Schedule powershellscript 
Change the -File location and when to run the file. 
```
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' ` -Argument '-NoProfile -WindowStyle Hidden -file "C:\Scripts\myScript.ps1" '
$trigger = New-ScheduledTaskTrigger -Daily -At 06:00
$principal = New-ScheduledTaskPrincipal -UserId "contoso\myuser" -RunLevel Highest
Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "MyTaskName" -Description "My script description"  -Principal $principal
```

## Script to move FSMO roles
```
https://github.com/qhrizz/Public/blob/master/Windows/Active%20Directory/Move-FSMO-Roles.ps1
```

## Deploy new domain controller 
```
https://github.com/qhrizz/Public/blob/master/Windows/Active%20Directory/New-Domaincontroller.ps1
```
## Delete Default apps in Windows 10
Deletes a few of them which for me are not neccesary
```
Start-Job -ScriptBlock {
$apps = @('Microsoft.3DBuilder','Microsoft.Office.OneNote','Microsoft.SkypeApp','Microsoft.Microsoft3DViewer','Microsoft.Getstarted','Microsoft.ZuneMusic','Microsoft.XboxApp','Microsoft.WindowsMaps','Microsoft.MicrosoftOfficeHub','Microsoft.WindowsAlarms','Microsoft.WindowsFeedbackHub','Microsoft.People','Microsoft.OneConnect','Microsoft.MicrosoftSolitaireCollection','Microsoft.ZuneVideo','Microsoft.BingWeather','Microsoft.XboxSpeechToTextOverlay','Microsoft.XboxIdentityProvider','Microsoft.XboxGameOverlay','Microsoft.Wallet','Microsoft.Messaging','Microsoft.Paint3D','Microsoft.XboxGamingOverlay')
foreach($app in $apps)
    {
        Write-Host "Tar bort $app"
        Get-AppxPackage $app | Remove-AppxPackage 
        }
    } | Wait-Job
    
``` 

## Show hidden files, trashbin confirmation and small taskbar
```
Start-Job -ScriptBlock {
#Show fileextension
Set-Location HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
Set-ItemProperty . HideFileExt "0"
Set-ItemProperty . Hidden "1"

#Trashbin delete confirmation
New-Item -Path HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies -Name Explorer
Set-Location HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
Set-ItemProperty . ConfirmFileDelete "1"

#Small taskbar
Set-Location HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
Set-ItemProperty . TaskbarSmallIcons "1"
Set-Location "$workingdir"
Start-Sleep -Seconds 10
Stop-Process -ProcessName explorer
} | Wait-Job
```

# Set Timezone to Europe 
```Powershell
Set-TimeZone -Id "W. Europe Standard Time"
```