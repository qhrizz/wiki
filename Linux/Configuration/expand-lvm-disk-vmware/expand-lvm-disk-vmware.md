--- 
title: Expand LVM volume on a machine runnning on vmware
parent: Configuration
grand_parent: Linux
tags: lvm, expand, vmware
---
# Expand LVM volume in vmware
In this guide the VM resides in Vmware and the operating system is Ubuntu 18.04, but should be somewhat applicable to other Linux OS’es

1. Begin with connecting to your server and type in `sudo fdisk -l`
If your disk has 4 partitions you cannot expand any more using this method (you can still attach a new disk and expand the existing LVM using this method) 
2. Sign on to your vCenter and edit settings of the virtual machine, increase the size of your disk
3. Locate which SCSI devices you have `sudo ls /sys/class/scsi_device/`
4. Then rescan the devices (or simply reboot the server if it is possible)
```
sudo echo 1 > /sys/class/scsi_device/1\:0\:0\:0/device/rescan
sudo echo 1 > /sys/class/scsi_device/2\:0\:0\:0/device/rescan
```

5.  If you re-run fdisk -l /dev/sda you can now see that the size of the disc is now the size you set (If you get GPT errors, try sudo parted -l)
6. Now run `fdisk /dev/sda`
7. Type `n` and press enter
Accept the default partition number 3 (or 2/4)
(The default first sector should be OK but you may have to compare to earlier partitions so you dont overwrite. ie the first sector should not be inside an existing one
If you get a GPT error with a missmatch error, you may run gdisk /dev/sda, w to write, press Y to correct the invalid sectors and write. Re-run fdisk /dev/sda)
8. Write the changes by typing `w`, enter
9. It is now time to extend the LVM with our new partition!
Create the physical volume with the newly created partition: `pvcreate /dev/sda3`
10. Type `pvs` and you should see the existing volumegroup, default in ubuntu is `ubuntu-vg`
11. Extend the volumegroup with the new volume: `vgextend ubuntu-vg /dev/sd3` 
12. After that it’s time to extend the volume to it’s new size: `lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv /dev/sda3` 
If you are having troubles finding the correct volumenames you can try `lvdisplay`
13. Resize the volume to expand to max size: `resize2fs /dev/ubuntu-vg/ubuntu-lv` and then verify the new disksize with `df -h`