---
title: Expand LVM volume on a fresh Ubuntu install
parent: Configuration
grand_parent: Linux
tags: lvm, expand
---
# Expand LVM volume after fresh install
Sometimes when you install a fresh Ubuntu 18.04 and 20.04 and choose the LVM option it doesn’t expand the volume and therefore the size remains 4GB even though the LVM is a lot bigger.

1. Connect to your machine and switch user to root sudo -s
2. First find out what your LVM volume is named with lvdisplay, the default for ubuntu is /dev/ubuntu-vg/ubuntu-lv
3. Then run lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv and the logical volume should extend to the maximum size available.
4. Then run resize2fs /dev/ubuntu-vg/ubuntu-lv to resize the filesystem to volumesize