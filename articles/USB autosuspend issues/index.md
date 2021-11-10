---
title: USB Autosuspend Feature Disconnects Active Devices
image: none
tags: ['USB', 'AM01 reconnect']
excerpt: USB Power Management and USB Autosuspend can sometimes suspend AM01. Here we will discuss how to disable USB Power Management for AM01.
createdAt: 2021-09-23 10:47:00
---


## Power Management and Autosuspend


Power Management is the practice of saving energy by suspending parts of a computer system when they aren't being used.  While a component/device is retained in a suspended state, it is in a nonfunctional low-power mode; it might even be turned off completely.  A suspended component can be resumed i.e returned to a functional full-power state when the OS or user needs to use it.

### USB Autosuspend

Almost all the modern OS and kernels implement Power Management strategies for USB devices to save energy. Unfortunately some USB devices have issues with this default setting and AM01 is one them. AM01 does not support Suspended state as it is designed to work 24/7. By default, OS/kernel will suspend USB devices that it thinks are idle:

> A device is idle whenever the kernel thinks it's not busy doing anything important and thus is a candidate for being suspended.  The exact definition depends on the device's driver; drivers are allowed to declare that a device isn't idle even when there's no actual communication taking place.

While AM01 implements power management requests as described in USB specification, in some rare cases the OS/kernel will ignore AM01 identification and device descriptors and will attempt to suspend a working AM01. Usually that leads to a number of device connect /disconnect notifications in the device log.

:::note
 As a workaround for this issue, when AM01 detects OS attempts to suspend it, it will reboot itself to appear as new USB device connected to the system. The only downside for this approach is that AM01 will discard all current jobs it could be working on before the reboot.
:::

:::note 
Starting from software version 1.0.4, all required system rules to prevent USB auto-suspend for AM01 devices are created automatically during the installation. No actions required if you're using the latest software version.
:::

### How to Check if USB Autosuspend is enabled

Run following command:
```bash
cat /sys/module/usbcore/parameters/autosuspend
```
Negative result value indicates that autosuspend feature is disabled in your system. Positive values indicate idle suspend device timeout in seconds.  

## Disable USB Autosuspend
### Temporary Disable USB Autosuspend System-Wide

The following command will disable USB Autosuspend feature for all connected devices until next system reboot:
```bash
sudo bash -c ‘echo -1 > /sys/module/usbcore/parameters/autosuspend’
```

### Permanently Disable USB Autosuspend System-Wide

Permanent solution to completely disable USB autosuspend feature is to pass additional kernel parameters upon system boot: `usbcore.autosuspend=-1`. The correct way to modify kernel parameters depends on your Linux distro and the bootloader used in your system. Here are the most common scenarios:

#### Raspberry Pi or Similar
Kernel parameters on Raspbian OS are typically located in `/boot/cmdline.txt` file. All you need to do is to add `usbcore.autosuspend=-1` so that `/boot/cmdline.txt` looks something like this:
```
dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 usbcore.autosuspend=-1 rootfstype=ext4 rootwait
```

#### Other Single Board Computer
If you're using a single-board computer from another manufacturer or using OS different than Raspbian, most likely kernel parameters can be found in `/boot/extlinux/extlinux.conf`. Example of `extlinux.conf` for RockPro64 board:
```
label RK3399_ROCKPRO64 linux
  kernel /Image
  devicetree /rk3399-rockpro64.dtb
  append earlycon=uart8250,mmio32,0xff1a0000 usbcore.autosuspend=-1 root=/dev/mmcblk0p4 rw rootwait
```

#### Desktop and Laptop Computers

It is common that laptop and desktop systems use Grub as a system bootloader. Edit the `/etc/default/grub` file and change the `GRUB_CMDLINE_LINUX_DEFAULT` line to add the `usbcore.autosuspend=-1` option:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash usbcore.autosuspend=-1"
```

### Flexible Solution to Disable Autosuspend

The most flexible solution would probably be to install and configure `tlp` [package](https://wiki.archlinux.org/title/TLP) which has numerous power management settings, including USB. More info about USB power management features of `tlp` can be found at [TLP Documentation](https://linrunner.de/tlp/settings/usb.html) 

Ubuntu/Debian users can install `tlp` with:
```bash
sudo apt install tlp
```
#### Disable USB Autosuspend System Wide with TLP

To permanently disable USB autosuspend with tlp, you will need to edit file `/etc/default/tlp` and modify `USB_AUTOSUSPEND` parameter there (or add it if it is not there for some reason):
```
USB_AUTOSUSPEND=0
```

#### Disable USB Autosuspend Per Device with TLP

To disable USB autosuspend, you will need to add USB device's VID:PID to the `USB_BLACKLIST` parameter. Example below disables autosuspend for AM01 devices:
```
USB_BLACKLIST="16d0:0e1e"
```