# UART - Console
## Hardware Preparation
![](https://github.com/tigerbox-reverse-engineering/tigerbox/raw/master/pics/Backside-UART.jpg)
Just connect your favorite 3.3V UART Interface to the marked touchpoints on the back. Reverse TX/RX and use the grey marked touchpad as GND.
* Voltage: 3.3V
* Baudrate: 115200 Baud
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
Currently unknown how to enable adb over USB