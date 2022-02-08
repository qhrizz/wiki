---
title: Set timezone in Ubuntu
parent: Setups
grand_parent: Linux
tags: linux, ubuntu, timezone
--- 
# Manually set timezone in Ubuntu 18.04 or 20.04
In this guide the OS is Ubuntu 18.04 but should work for most operating systems  :)
1. Begin with listing available timezones with: `sudo timedatectl list-timezones`
2. Then set the timezone with (for example): `sudo timedatectl set-timezone Europe/Stockholm`