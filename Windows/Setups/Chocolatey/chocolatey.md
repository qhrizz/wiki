---
title: Setup Chocolatey and common apps
parent: Setups
grand_parent: Windows
tags: windows, chocolatey
---
# Chocolatey

## Install Chocolatey
Open Powershell with administrative rights

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

# Install Firefox
```
choco install firefox -y 
```

# Install Chrome 
```
choco install googlechrome -y 
```

# Install 7zip
```
choco install 7zip.install -y 
```

# Install Notepad++
```
choco install notepadplusplus.install -y 
```

# Install Git
```
choco install git.install -y 
```

# Install VS Studio Code
```
choco install vscode -y 
```

# Install Putty
```
choco install putty -y 
```

# Install Youtube-DL
```
choco install youtube-dl -y 
```

# Install FFMPEG
```
choco install ffmpeg -y 
```

# Install ShareX
```
choco install sharex -y 
```

# Install Powershell Core
```
choco install powershell-core -y 
```

# Install Spotify
```
choco install spotfiy -y
```

# Install Steam
```
choco install steam -y 
```

# Install Windows Terminal
```
choco install microsoft-windows-terminal
```

# Install velero cli
```
choco install velero -y 
```

# Installa kubernetes cli
```
choco install kubernetes-cli -y 
```