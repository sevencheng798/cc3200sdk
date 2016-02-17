CC3200 Linux development environment with OpenOCD
================================================================================
>**Note:**

> - This document contains a HOWTO on developing applications for the TI CC3200
Launchpad under Ubuntu 64bit 14.04. It is a developing project, but all information contained
should work and has been tested on my Ubuntu system. If you feel that
something is missing, can be improved or is wrong, please let me know.

> - Libs and examples are extracted from TI CC3200SDK v1.1.0. The hardware is tested on CC3200-LAUNCHXL
The SOP2 should be 100, service pack is by defult.

Setup
---------------------
Install GNU Tools for ARM Embeded Processor: `gcc-arm-none-eabi`[Click here](https://launchpad.net/~terry.guo/+archive/ubuntu/gcc-arm-embedded)


Install OpenOCD for onchip debugging: `openocd` (v0.7)

The launchpad is based on an FTDI chip, and current Linux kernels provide very good
support for it. If the kernel module does not kick in automatically, because TI did
change VID and PID code. Run the command below (for kernel >= 3.12)

      modprobe ftdi_sio
      echo 0451 c32a > /sys/bus/usb-serial/drivers/ftdi_sio/new_id

[Reference](https://community.spark.io/t/cc3200-network-processor-information-station/5348/60)

The board should have enumberated as `/dev/ttyUSB{0,1}`. We will use `ttyUSB1` for UART
backchannel


Build your first example
---------------------
Take `example/getting_started_with_wlan_station` as example.
Go to gcc, type `make -f Makefile`, you will see the `wlan_station.axf` generated under `exe` folder.
Copy this file to `/tools/gcc_scripts`.

First, make sure you got openocd setup by running `openocd -f cc3200.cfg`, you should get
output like

      Open On-Chip Debugger 0.7.0 (2013-10-22-08:31)
      Licensed under GNU GPL v2
      For bug reports, read
        http://openocd.sourceforge.net/doc/doxygen/bugs.html
      Info : only one transport option; autoselect 'jtag'
      adapter speed: 1000 kHz
      cc3200_dbginit
      Info : clock speed 1000 kHz
      Info : JTAG tap: cc3200.jrc tap/device found: 0x0b97c02f (mfg: 0x017, part: 0xb97c, ver: 0x0)
      Info : JTAG tap: cc3200.dap enabled
      Info : cc3200.cpu: hardware has 6 breakpoints, 4 watchpoints

Then, begin to debug with ` sudo arm-none-eabi-gdb -x gdbinit wlan_station.axf`. Typing continue when got 
input prompt, you will connect the AP you specified in '\TI\CC3200SDK_1.1.0\cc3200-sdk\example\common\commom.h'.

```
		 *************************************************
		           CC3200 WLAN STATION Application       
		 *************************************************



Host Driver Version: 1.0.0.10
Build Version 2.0.7.0.31.0.0.3.0.1.1.8.8
Device is configured in default state 
Device started as STATION 
[WLAN EVENT] STA Connected to the AP: EIT ,BSSID: e8:de:27:67:4d:bd
[NETAPP EVENT] IP Acquired: IP=192.168.1.174 , Gateway=192.168.1.1
Connection established w/ AP and IP is aquired 
Pinging...! 
Device pinged both the gateway and the external host 
WLAN STATION example executed successfully 
```

Flash your example from Ubuntu or BeagleBone Black
---------------------
Check tools folder for further details.


