---
title: Install WSL2
parent: Setups
grand_parent: Windows
tags: windows, wsl2
---
Just a heads up, Faceit anticheat client does not like this...


# Install WSL2

1. Open powershell wit administrative rights and run
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2. Then download the WSL 2 kernel from Microsoft https://aka.ms/wsl2kernel

3. Open Powershell again with administrative rights and run 
```
wsl --set-default-version 2
```
To set the default WSL version

Download a dist from MS Store, for example

- Ubuntu 20.04 https://www.microsoft.com/store/apps/9n6svws3rx71
- Kali Linux https://www.microsoft.com/store/apps/9PKR34TNCV07 
- Debian https://www.microsoft.com/store/apps/9MSVKQC78PK6


# wslconfig to limit ram usage
1.  Create `%UserProfile%\.wslconfig`
2. Paste 
```
[wsl2]
memory=4GB
swap=0
localhostForwarding=true

```


# Manage WSL2 instances

### List running WSL instances
```
wsl --list --running
```
### List all instances
```
wsl --list --all
```
### Stop WSL instance 
```
wsl --terminate [NAME OF INSTANCE]
``` 
### Start WSL instance
```
wsl -d [NAME OF DISTANCE]
```
### Show help 
```
wsl --help
```

### Uninstall / unregister WSL instance
```
wsl --unregister [Distribution Name]
```
Notice that this will remove ALL settings, files etc related to the distro. 
To reinstall, you must download the distro again from the Microsoft store!