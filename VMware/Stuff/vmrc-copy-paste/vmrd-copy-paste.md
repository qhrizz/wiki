---
title: Enable copy and paste for VMRC
parent: Stuff
grand_parent: VMware
tags: vmware, vmrc
---
# VMRC Copy paste
1. Power off the VM
2. Edit the VM and go to VM Options and expand Advanced -> Edit configuration
3. 
```
isolation.tools.copy.disable          FALSE
isolation.tools.paste.disable         FALSE
isolation.tools.setGUIOptions.enable  TRUE
```
4. Start the VM again