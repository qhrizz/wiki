---
title: Create macOS Monterey iso
parent: Setups
grand_parent: macOS
tags: macOS, iso
---
# Create a macOS Monterey ISO

Begin with download the "Package" from the App store
1. Create the disk
```
sudo hdiutil create -o /tmp/Monterey -size 16384m -volname Monterey -layout SPUD -fs HFS+J
```
2. Mount it
```
sudo hdiutil attach /tmp/Monterey.dmg -noverify -mountpoint /Volumes/Monterey
```
3. Install Monterey to the DMG
```
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/Monterey --nointeraction
```
4. Unmount the disk
```
sudo hdiutil eject -force /Volumes/Install\ macOS\ Monterey/
``` 
5. Convert the DMG to ISO
```
hdiutil convert /tmp/Monterey.dmg -format UDTO -o ~/Desktop/Monterey
mv -v ~/Desktop/Monterey.cdr ~/Desktop/Monterey.iso
sudo rm -fv /tmp/Monterey.dmg
```