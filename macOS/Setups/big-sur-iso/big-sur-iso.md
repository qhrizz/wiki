---
title: Create macOS Big Sur iso
parent: Setups
grand_parent: macOS
tags: macOS, iso
---
# Create Big Sur ISO
Begin with download the "Package" from the App store
1. Create the disk
```
sudo hdiutil create -o /tmp/BigSur -size 16384m -volname BigSur -layout SPUD -fs HFS+J
```
2. Mount it
```
sudo hdiutil attach /tmp/BigSur.dmg -noverify -mountpoint /Volumes/BigSur
```
3. Install Big Sur to the DMG
```
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/BigSur --nointeraction
```
4. Unmount the disk
```
sudo hdiutil eject -force /Volumes/Install\ macOS\ Big\ Sur/
``` 
5. Convert the DMG to ISO
```
hdiutil convert /tmp/BigSur.dmg -format UDTO -o ~/Desktop/BigSur
mv -v ~/Desktop/BigSur.cdr ~/Desktop/BigSur.iso
sudo rm -fv /tmp/BigSur.dmg
```