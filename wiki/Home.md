Welcome to the tigerbox wiki!

This wiki contains all tigerbox related information we gathered.

# Master plan for unlocking

So let's say we have a [Tigerbox](https://tiger.media/tigerbox-touch/), and we
want to use it for something more than the german audiobooks for kids.

What are the general steps we have to take? You probably already have that in your mind:

* As the box is an [Android](https://en.wikipedia.org/wiki/Android_(operating_system))
 device, we need to enable a way of sideloading and installing Android applications.

* When we're able to install applications, we should make some kind of menu-based launcher
 to be started in place of the fullscreen Tigerbox app.

* After the menu starts instead of Tigerbox app, we can install further applications and use
 them to play any music or watch any videos.

# Unlocking in more detail

Now we can think about detais of each point in the plan.

## Enable a way of sideloading and installing apps

Android devices can be controlled using [Android Debug Bridge](https://developer.android.com/studio/command-line/adb).
The ADB can use USB or Wi-fi network to talk to the target device from any PC.
But both these ways of communication are normally disabled on Tigerbox - we
need to enable at least one of them.

While it is definitely possible to hack your way into the Tigerbox without
opening it, as of now noone bothered to create such automated solution.
It is a nieche product after all.

So unless you are able to develop such solution yourself, you'll have to use
your screwdriver and soldering station:

* [[Open the tigerbox|Open the tigerbox]]

* [[Make UART connection to enable ADB|UART ADB Root]]

After this is done, you shouldn't have to access the internal connections
anymore - you can close it.

## Install launcher, store and other apps

Now it is just a matter of utilizing the ADB connection to [[Install Launcher and Store]].
It might be worth to also install some shared services, so that apps can use
them.

## Use the apps through the menu

No tutorial needed for that.
