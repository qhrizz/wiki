---
title: Install Kali in WSL2
parent: Setups
grand_parent: Windows
tags: windows, wsl2, kali
---

# Offical Docs
https://www.kali.org/docs/wsl/win-kex/ 
https://www.kali.org/docs/wsl/win-kex-win/ 

See also [Manage WSL2 instances](/windows/manage-wsl2)
# Enable WSL
1. Open Powershell with administrative rights and run
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2. Reboot
3. Then download the WSL 2 kernel from Microsoft https://aka.ms/wsl2kernel
4. Open Powershell again with administrative rights and run 
```
wsl --set-default-version 2
```
To set the default WSL version
5. Install Kali linux from the Microsoft Store: https://www.microsoft.com/store/apps/9PKR34TNCV07
7. Finish the install which pops up in a separate window. Add a user and set a password
8. Install Kex with
```
sudo apt install -y kali-win-kex
```
9. The default install is minimal, to install other tools included in the normal iso, run 
```
sudo apt install -y kali-linux-large
```
## RDP Mode
```
kex --esm
```


## Windowed Mode
In the kali shell, run

```
kex --win
```

This will prompt for a VNC password, add one and remember it.

Once connected to the VNC session you will see the kali desktop. Press F8 to exit fullscreen or to edit other settings. 

## Stop kex
Open a terminal and type

```
kex --stop
```