---
title: Setup LAPS
parent: Setups
grand_parent: Windows
tags: windows, laps
---

# LAPS
1. On a domain computer, download LAPS msi file https://www.microsoft.com/en-us/download/details.aspx?id=46899. LAPS.x64.msi should be OK
2. Run the msi and enable the features
- Powershell Module
- GPO Editor Template
3. Launch Powershell with administrative rights and run `Import-module AdmPwd.PS`
4. The next command needs to be run as network schema admin `Update-AdmPwdADSchema`
Everything should be `Success`
5. The next step is to configure the GPO. The policy ADMX template files are located under `C:\Windows\PolicyDefinitions\AdmPwd.admx` on the machine you installed the MSI package on. If needed you can copy these to your domaincontrollers or Windows admin server (preferably)
6. Open the Group Policy manager mmc, create a new GPO and go to Computer configuration -> Policies -> Administrative Templates -> LAPS and configure it to your liking (or company policy)
7.  It is now time to set the permissions for LAPS
Replace the OU where you have your clients/servers
`Set-AdmPwdComputerSelfPermission -OrgUnit "OU=Computers,OU=Qnet,DC=potato,DC=domain,DC=se"`
We can also create a readonly group (for servicedesk) that can read the properties
`Set-AdmPwdReadPasswordPermission -OrgUnit "OU=Computers,OU=Qnet,DC=potato,DC=domain,DC=se" -AllowedPrincipals "laps_reader"`
And a for the admins
`Set-AdmPwdResetPasswordPermission -OrgUnit "OU=Computers,OU=Qnet,DC=potato,DC=domain,DC=se" -AllowedPrincipals "laps_admins"`
8.  For the client to be able to update the password into AD they need the DLL file. There are two popular ways, one is copying the AdmPwd.dll to all computers %SystemRoot%\SysWOW64\AdmPwd.dll but I like Powershell app deploy toolkit. I’ll link it here [psapp-deploy_lapsclient.zip](psapp-deploy_lapsclient.zip)
9. Edit the LAPS policy to deploy it with a GP -> Powershellscript (I stored the unpacked zipfile in \\domain\sysvol\domain\Deploy)
10. Now restart the servers/clients for the installation to take place. Checking installed programs should now show “Local Administrator Password Solution”
11. On the computer with the management tools you can now run `Get-AdmPwdPassword -ComputerName "MyComputer"`
You may also run `Get-AdmPwdPassword -ComputerName "MyComputer" | Select-Object ComputerName,Password` or view it in the thick client or in the attribute editor
12. Done :)