--- 
title: Repair Windows Domain relationshop
parent: Troubleshooting
grand_parent: Windows
tags: windows, troubleshooting
---

# Repair domain relationship

1. Open an elevated powershell shell
2. Test-ComputerSecureChannel -Repair -Credential (Get-Credential)
3. If it returns True, it should be OK
