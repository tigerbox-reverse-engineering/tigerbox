# Preparation

Please use the [UART-ADB UART tutorial](https://github.com/tigerbox-reverse-engineering/tigerbox/wiki/UART-ADB-Root)
to gain ADB access.

# Make Android GUI useable

We need the ability to install and start apps from the screen, like on any Android device.

## Install F-Droid

It is an app store, which is easy to get and has a lot of software we can use.
It has very little dependency on existing software, so will work on any Android,
no matter how much cut down.

Download [F-Droid APK](https://f-droid.org/F-Droid.apk) and install it via ADB.

```
adb install F-Droid.apk
```

## Install OpenLauncher

Now we need to have a menu with applications - in other words, a launcher.

Run _F-Droid_, and install _OpenLauncher_.

```
adb shell monkey -p 'org.fdroid.fdroid' -v 5
```

If the launcher isn't in the foreground, run it directly.

```
adb shell monkey -p 'com.benny.openlauncher' -v 5
```

Don't forget to set the _OpenLauncher_ as standard launcher in _Settings_.
Otherwise, it will start the _Tigerbox App_.

## Install VirtualSoftKeys

As the device only has a power button, you will need some kind of virtual buttons.
[VirtualSoftKeys](https://f-droid.org/packages/tw.com.daxia.virtualsoftkeys/)
is a good choice, and available in _F-Droid_ store.


# Additional notes

## Missing GUI elements

The statusbar and the navbar are deactivated.

## Starting software via adb

First get a list of your available packages, then run it (replace `<PACKAGENAME>`)

```
adb shell pm list packages
adb shell monkey -p '<PACKAGENAME>' -v 500
```

## Use scrcpy as remote control.

[scrcpy](https://github.com/Genymobile/scrcpy) makes remote controlling the
device very convient. Just connect your device via adb and start scrcpy.

## Working Software

* [F-Droid](https://f-droid.org/F-Droid.apk) - F-Droid - Store
* [Aurora Store](https://f-droid.org/packages/com.aurora.store/) - F-Droid - Play Store
* [OpenLauncher](http://f-droid.org/packages/com.benny.openlauncher/) - F-Droid - Launcher
* [NewPipe](https://f-droid.org/packages/org.schabi.newpipe/) - F-Droid - YouTube GUI
* [VirtualSoftKeys](https://f-droid.org/packages/tw.com.daxia.virtualsoftkeys/) - F-Droid - Hardware button replacement

## Non-Working Software

* YouTube - Aurora Store - Crashes because of no Google Services
* Spotify - Aurora Store - Just crashes
