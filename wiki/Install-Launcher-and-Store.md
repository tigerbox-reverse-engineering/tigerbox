# Preparation

Please use the [[UART-ADB root tutorial|UART ADB Root]] to gain ADB access.

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

# Supplemental Android improvements

## Add more Stock Packages

The Android you have is missing some standard packages which are used by other
Apps. Not having these stock dependencies will make many applications not work.

Where to get these packages from, you ask? You extract them from a stock image,
preferably matching the Android version and SoC which you have in your _Tigerbox_.
And what you have, you can check in _Settings_ -> _Device info_.

If your Android is 5.1.1, and chip is rk312x, then you should be able to use the
already extracted packages:

* selection from images of some RK312x Tablets:
  [Download from Mega] https://mega.nz/file/S3B3iJAB#AzEKVNInkoZ3dvBPcdakYnN2Rh35uccJ4TCe-2GGJ4A

The APK files could be installed by `adb install --user 0 <FILENAME>`, but you would
notice that some files are refusing to install unless modded, and in general -
these packages should be really maintained by the Android system, not by the user.

To include them into the system-maintained pool, you need place them into folders
`/system/priv-app` and `/system/app`. Packages placed there are automatically
installed on system reboot, as system-maintained packages.

### Add Stock Packages to 'priv-app'

First, copy the files to a temporary folder on your device.

```
adb shell mkdir /sdcard/temp-priv-app
adb push priv-app/* /sdcard/temp-priv-app
```

Now, log in to the device, and get _Super User_ privileges.

```
adb shell
su
```

Then, you can move the folders into final location, and remove the temporary copy.


```
cp -r /sdcard/temp-priv-app/* /system/priv-app/
rm -rf /sdcard/temp-priv-app
```

While still in the ADB shell with elevated priveleges, change attributes of the folders
and files you copied. This is neccessary, because this version of Android
is very strict about attributes, and will ignore the packages which do not match.

```
chmod 755 /system/priv-app/CalendarProvider
chmod 644 /system/priv-app/CalendarProvider/*

chmod 755 /system/priv-app/Contacts
chmod 644 /system/priv-app/Contacts/*

chmod 755 /system/priv-app/ContactsProvider
chmod 755 /system/priv-app/ContactsProvider/*

chmod 755 /system/priv-app/DownloadProvider
chmod 644 /system/priv-app/DownloadProvider/*

chmod 755 /system/priv-app/MusicFX
chmod 644 /system/priv-app/MusicFX/*

chmod 755 /system/priv-app/TelephonyProvider
chmod 644 /system/priv-app/TelephonyProvider/*

chmod 755 /system/priv-app/TeleService
chmod 644 /system/priv-app/TeleService/*
```

You can exit the shell by typing `exit` command, twice. Or make the device restart
and install the new packages by typing `reboot`.

After the packages are installed, you can switch from _OpenLauncher_ to
the standard _Android Launcher_, if you prefer. The standard one is a bit
more responsive, but both are useable.

If the _Android Launcher_ doesn't show wallpaper, then execute in ADB shell:
`pm enable com.android.systemui`. Effects are immediate.

### Add Stock Packages to `app`

We will do this using the same procedure as we did for `priv-app`.

First, copy the files to a temporary folder on your device.

```
adb shell mkdir /sdcard/temp-app
adb push app/* /sdcard/temp-app
```

Now, log in to the device, and get _Super User_ privileges.

```
adb shell
su
```

Then, you can move the folders into final location, and remove the temporary copy.


```
cp -r /sdcard/temp-app/* /system/app/
rm -rf /sdcard/temp-app
```

While still in the ADB shell with elevated priveleges, change attributes of the folders
and files you copied.

```
chmod 755 /system/app/Browser
chmod 644 /system/app/Browser/*

chmod 755 /system/app/BrowserProviderProxy
chmod 644 /system/app/BrowserProviderProxy/*

chmod 755 /system/app/Calculator
chmod 644 /system/app/Calculator/*

chmod 755 /system/app/Calendar
chmod 644 /system/app/Calendar/*

chmod 755 /system/app/ConfigUpdater
chmod 644 /system/app/ConfigUpdater/*

chmod 755 /system/app/DeskClock
chmod 644 /system/app/DeskClock/*

chmod 755 /system/app/DocumentsUI
chmod 644 /system/app/DocumentsUI/*

chmod 755 /system/app/DownloadProviderUi
chmod 644 /system/app/DownloadProviderUi/*

chmod 755 /system/app/Exchange2
chmod 644 /system/app/Exchange2/*

chmod 755 /system/app/Launcher3
chmod 644 /system/app/Launcher3/*

chmod 755 /system/app/MediaShortcuts
chmod 644 /system/app/MediaShortcuts/*

chmod 755 /system/app/MusicLocal
chmod 644 /system/app/MusicLocal/*

chmod 755 /system/app/PartnerBookmarksProvider
chmod 644 /system/app/PartnerBookmarksProvider/*

chmod 755 /system/app/RkApkInstaller
chmod 644 /system/app/RkApkInstaller/*

chmod 755 /system/app/RkExplorer
chmod 644 /system/app/RkExplorer/*

chmod 755 /system/app/SoundRecorder
chmod 644 /system/app/SoundRecorder/*
```

Finally, make the device restart and install the new packages by typing `reboot`.

After reboot, you should see new apps in the menu: _Calculator_, _Calendar_, _Music_, _File Explorer_.

Since you've installed `com.android.providers.downloads.ui`, `com.android.documentsui`
and `com.android.browser.provider`, you should now be able to save
files locally in various applications, ie. music and videos, and play them later
even if there is no wifi connection.

## Install Lucid Browser

Web browser will be handy. [Lucid Browser](https://f-droid.org/packages/com.powerpoint45.lucidbrowser/)
is fast, and works with minimal amount of dependencies. The video playback
functionality works properly, so you can use any video services through it
when _NewPipe_ isn't enough. Available in _F-Droid_ store.

## Install commonly used apps

Some apps have a button which opens e-mail or calendar. Even if you don't need
these functionalities, you probably do not want such apps to crash. So, install
such apps from the store.

For example, [SimpleEmail](https://f-droid.org/packages/org.dystopia.email/)
fills the post for e-mail app quite well.

Also, you will probably want a video player - [NewPipe](https://f-droid.org/packages/org.schabi.newpipe/).
While not a dependency for other software, it is nice to have for this kind of device.

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
re-added (see [Supplemental Android improvements](#supplemental-android-improvements).
Others are harder, and still require figuring out. If you see a _Java_ crash
dump starting with:

```
android.content.ActivityNotFoundException: No activity found to handle Intent { ... }
```

then the issue is just that - lack of handlers. For what action the handler is missing,
that is explained within curly braces of the message above.

## Missing Google services

The Google services are not very usable without the _Google Play Store_. And to
make _Google Play Store_ work, you need _signature spoofing_ support on your
Android system.

Manually adding _signature spoofing_ is complicated. There are tools which allow
to do that easily - but these are installed within [custom recovery](#custom-recovery).

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

## Starting software via ADB

In case you accidentally revert to original launcher, or for various other
reasons, you may sometimes want to start an app without using the screen.

To start an app, first get a list of your available packages, then run it
 (replace `<PACKAGENAME>`)

```
adb shell pm list packages
adb shell monkey -p '<PACKAGENAME>' -v 500
```

## Finding issues with installation of system packages

If you want to add even more system-maintained packages, or for some reason there is
an issue with adding the ones from [Supplemental Android improvements](#supplemental-android-improvements)
chapter, you may want to look at logs which may shed light on what is wrong.

If you are installing a package which doesn't add icon to the Launcher (ie. it's a
Provider package), and you want to make sure it got installed, you should check:


```
adb shell dumpsys package
```

If the name of your package appears anywhere above `Package warning messages:`
in that log, then it was successfully installed and it provides some functionality.

And if it only appears in the `Package warning messages:`, then the warning might
tell you what exactly is a problem with installation.

## Finding issues with crashing app

If any app or service is crashing, the information shown in GUI is not very
informative, and often deceptive (ie. it often says _Android Core_ crashed,
even though the issue is with specific application, not with the _Android Core_.

To get more descriptive error message, run:

```
adb shell logcat
```

After the crash happens, break the command (ctrl+c), and look at the messages
finding errors.

## Use scrcpy as remote control

The tool [scrcpy](https://github.com/Genymobile/scrcpy) makes remote controlling the
device very convient. Just connect your device via ADB and start _scrcpy_.

## Working Software

* [F-Droid](https://f-droid.org/F-Droid.apk) - F-Droid - Store
* [Aurora Store](https://f-droid.org/packages/com.aurora.store/) - F-Droid - Play Store
* [OpenLauncher](http://f-droid.org/packages/com.benny.openlauncher/) - F-Droid - Launcher
* [NewPipe](https://f-droid.org/packages/org.schabi.newpipe/) - F-Droid - YouTube GUI
* [VirtualSoftKeys](https://f-droid.org/packages/tw.com.daxia.virtualsoftkeys/) - F-Droid - Hardware button replacement

## Non-Working Software

* YouTube - Aurora Store - Crashes because of no Google Services
* Spotify - Aurora Store - Just crashes
