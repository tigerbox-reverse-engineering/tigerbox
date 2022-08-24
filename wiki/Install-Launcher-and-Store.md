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

The _statusbar_ at top and the _navbar_ at bottom (collectively known as _systembar_)
are deactivated. We don't really know where these were disabled - there surely
is a way to re-enable, but it isn't known. Replacing _SystemUI_ with stock one
doesn't bring the bars back. So maybe it's disabled in _Framework_? Or maybe
there is a `build.prop` property to enable these? More experiments needed.

## Missing Action handlers

The Android build used was severly cut down. It is missing several provider
packages, and other commonly used parts of Android. Some of these can be easily
re-added (see [Supplemental Android improvements](#supplemental-android-improvements),
others are harder. Others still require figuring out. If you see a Jave crash
dump starting with:

```
android.content.ActivityNotFoundException: No activity found to handle Intent { ... }
```

then the issue is just that - lack of handlers. For what the handler is missing,
that is explained within curly braces of the message above.

## Missing Google services

The Google services are not very usable without the _Google Play Store_. And to
make _Google Play Store_ work, you need _signature spoofing_ support on your
Android system.

Manually adding _signature spoofing_ is complicated. There are tools which allow
to do that easily - but these are installed within [custom recovery](#custom-recovery].

In fact, if you make _custom recovery_ work, you can also install all the google
services through _microG_ which is also installed from within _custom recovery_.

This will give you access to _Google Maps_, _Gmail_, _Youtube_, and all the other
services provided by Google and required by a lot of other Apps.

## Custom recovery

For popular Android devices, there is a _custom recovery_ available. It is a
replacement for a recovery partition, which can be activated by starting the
device with specific buttons combination pressed. For example, [TWRP](https://twrp.me/)
is one of such recoveries.

_Custom recovery_ allows installing many hacks created by the community.
For example, there are packages to tweak Android UI, or enable _signature
spoofing_ required for adding google services, or rooting (though you do not
need rooting on _Tigerbox_, it just gives you full access).

We haven't heared any reports of successful _TWRP_ installations on _Tigerbox_.
You can be the first.

## Starting software via adb

First get a list of your available packages, then run it (replace `<PACKAGENAME>`)

```
adb shell pm list packages
adb shell monkey -p '<PACKAGENAME>' -v 500
```

## Use scrcpy as remote control.

[scrcpy](https://github.com/Genymobile/scrcpy) makes remote controlling the device very convient. Just connect your device via adb and start scrcpy.

## Working Software

* [F-Droid](https://f-droid.org/F-Droid.apk) - F-Droid - Store
* [Aurora Store](https://f-droid.org/packages/com.aurora.store/) - F-Droid - Play Store
* [OpenLauncher](http://f-droid.org/packages/com.benny.openlauncher/) - F-Droid - Launcher
* [NewPipe](https://f-droid.org/packages/org.schabi.newpipe/) - F-Droid - YouTube GUI
* [VirtualSoftKeys](https://f-droid.org/packages/tw.com.daxia.virtualsoftkeys/) - F-Droid - Hardware button replacement

## Non-Working Software

* YouTube - Aurora Store - Crashes because of no Google Services
* Spotify - Aurora Store - Just crashes
