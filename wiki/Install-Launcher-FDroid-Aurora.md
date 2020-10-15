# Preparation
Please use the [UART-ADB UART tutorial](https://github.com/tigerbox-reverse-engineering/tigerbox/wiki/UART-ADB-Root) to gain ADB access.
# Get Android Access
## Install F-Droid
Download [F-Droid APK](https://f-droid.org/F-Droid.apk) and install it via adb.
```
adb install F-Droid.apk
```
## Install OpenLauncher
Run F-Droid and install OpenLauncher
```
adb shell monkey -p 'org.fdroid.froid' -v 5
```
If the launcher isn't in the foreground, run it directly.
```
adb shell monkey -p 'com.benny.openlauncher' -v 5
```
Don't forget to set the OpenLauncher as standard launcher. Otherwise it will start the Tigerbox App.
## Install VirtualSoftKeys
As the device only has a power button you will need some kind of virtual buttons.
[VirtualSoftKeys](https://f-droid.org/packages/tw.com.daxia.virtualsoftkeys/)
# Additional notes
## Missing GUI elements
The statusbar and the navbar are deactivated.
## Starting software via adb
First get a list of your available packages, then run it (replace <PACKAGENAME>)
```
adb shell pm list packages
adb shell monkey -p '<PACKAGENAME>' -v 500
```
## Use scrcpy as remote control.
[scrcpy](https://github.com/Genymobile/scrcpy) makes remote controlling the device very convient. Just connect your device via adb and start scrcpy.
## Working Software
* [OpenLauncher](http://f-droid.org/packages/com.benny.openlauncher/) - F-Droid - Launcher
* [NewPipe](https://f-droid.org/packages/org.schabi.newpipe/) - F-Droid - YouTube GUI
## Non-Working Software
* YouTube - Aurora Store - Just crashes
* 