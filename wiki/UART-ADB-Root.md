# UART - Console

## Hardware Preparation

![](https://github.com/tigerbox-reverse-engineering/tigerbox/raw/master/pics/Backside-UART.jpg)

Just connect your favorite 3.3V UART Interface to the marked test pads on the back. Reverse TX/RX and use the grey marked test pad as GND.

* Voltage: 3.3V
* Baudrate: 115200 Baud
* Parity: None
* Flow control: XON/XOFF

## Serial Console - Root / SU

Feel free here. Just type `su` and gain root. Done...

## FIQ Debugger

It may be possible to get into the FIQ debugger while booting and sending something in the right moment.

# ADB

## via WiFi

### Preparation - UART
You will need to enable WiFi ADB via UART. This pr
```
setprop service.adb.tcp.port 5555
stop adbd
start adbd
```
### ADB Connection
```
adb connect <tigerbox-ip>:5555
```

## via USB

To enable ADB over USB, you need to make the following lines go into `/system/build.prop`:
```
persist.service.adb.enable=1
persist.service.debuggable=1
persist.sys.usb.config=mtp,adb
```
Stock image has this configuration instead:
```
persist.sys.usb.config=charging
```

So it's as easy as modifying one line and additng two new ones to one file? Yes.
Though it's not always easy, tigerbox doesn't protect your device from you.

First, check if the partition is mounted to `/system` with write enabled.
To do that, execute `mount` to list all mount points, and find the line for
that partition. Make sure it's marked as read-write by `rw` attrib, ie:
```
/dev/block/platform/1021c000.rksdmmc/by-name/system /system ext4 rw,seclabel,noatime,nodiratime,noauto_da_alloc,data=ordered 0 0
```
If you see `ro` instead, remount the partition.

Then, check which properties are set in your `build.prop`. On a stock image, I got:
```
shell@rk312x:/ $ cat /system/build.prop | grep persist
persist.tegra.nvmmlite = 1
persist.sys.caration.standby=60000
persist.sys.caration.antiaddic=false
persist.sys.timezone=Europe/Amsterdam
persist.sys.boot.check=false
persist.sys.strictmode.visual=false
persist.sys.usb.config=charging
persist.sys.dalvik.vm.lib.2=libart.so
```
Now we need a text editor. The Android image has `busybox`, so we can use the utilities inside.
One option is visual editing with `vi`, my preference is command line editing with `sed`:
```
su
busybox sed -i 's/^\(persist[.]sys[.]usb[.]config\)=.*$/\1=mtp,adb/' /system/build.prop
busybox sed -i '/^persist[.]sys[.]usb[.]config=.*$/a persist.service.debuggable=1' /system/build.prop
busybox sed -i '/^persist[.]sys[.]usb[.]config=.*$/a persist.service.adb.enable=1' /system/build.prop
```
After you reboot the device, it will report as adb device and media device.
Note: if you damage `build.prop`, fixing it might be tricky. Be careful.

You will have to make sure the specific USB identifier (Vendor ID and Product ID)
is treated as "Composite ADB Interface" on your PC. If using Windows, either modify
ADB driver from Google so that it recognizes your device, or force install the
Google ADB driver on unrecognized "ADB Device" in "Device Manager".

## via WIFI
May be possible with
https://github.com/tintinweb/pub/tree/master/pocs/cve-2017-13208


# via USB _Android Recovery_ without opening

 ## Currently only works on Linux or Windows Subsystem for Linux 2 with adb installed.

Use a Paperclip to press it in the RESET hole right from the USB Connector. While doing this press the on/off button to boot in Android Recovery. Use `adb pull /dev/block/mmcblk0p12 /patch/to/file`. And mount over `sudo mount -t ext4 /patch/to/file/mmcblk0p12 /mount/patch/` `nano /mount/patch/build.prop` and add
```
persist.service.adb.enable=1
persist.service.debuggable=1  
```

and change `persist.sys.usb.config=charging` to `persist.sys.usb.config=mtp,adb` 
