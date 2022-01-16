---
title: Development and Debug Software Builds
tags: ['Software', 'atomminer-cli']
excerpt: How to install and test recent software builds on your system. Try new features and fixes before they are released.
createdAt: 2021-12-13 16:57:00
---


## Development and Debug Software Builds

We're publishing development and debug builds every time changes are made to the application source code. Moreover, we do provide native debug builds with extended logging capabilities and crash handlers. Debug builds can be really helpful when user is running a non-standard OS or have issues with hardware and/or software configuration.

All builds are being built and published online using our private build servers upon each git commit using CI/CD practices. Build and deploy process is not usually tested across every supported platform and architecture thus some of the published binary might not work properly (that's why these are development builds).

Actual commit number, software revision and update dates are automatically published [online](https://static.atomminer.com/software/dev/version.txt). Binary files and helper install scripts can be downloaded for your architecture here: [https://static.atomminer.com/software/dev/](https://static.atomminer.com/software/dev/)

### Release vs Debug builds

We're releasing both debug and release builds in our development repository at the same time. 
 - release builds are built in the way we would build any official release;
 - debug builds are usually built with extended logging support, most logging options turned on by default and extended memory tracking in place;

I simple words, we do not recommend using debug versions unless you absolutely have to. Debug versions are larger and slower due to enormous overhead added by memory tracing, event handlers and various crash loggers.

Release builds should continue working even after they detect a potential problem while debug builds will create log files and treat such conditions as critical, even if they are not. Debug version of the `atomminer-cli` will stop and create log file in such case.

### Where to Get Latest Development Build

As mentioned before, all the binary files are available for download at [https://static.atomminer.com/software/dev](https://static.atomminer.com/software/dev/) 

### Development Build Requirements

`atomminer-cli` is not location dependent itself, however it does require read/write access to the following folders:
- `/var/atomminer/firmware` - to download and access FPGA bitstreams;
- `/var/atomminer/cache` - to store internal cached data across runs;
- `/var/atomminer/log` - only used by debug builds as an output for crash and problems log.

`atomminer-cli` can be started from any folder and it will search for configuration file in the following order: the same folder it is located in and `/var/atomminer/atomminer.conf` if conf file in the current folder is missing or contains errors. Thus giving you the opportunity to have multiple versions of `atomminer-cli` in your system with different config sets. 
Please note that only single instance of `atomminer-cli` will be working at any single time regardless of its version on the first come - first serve basis.

### Installing Development Version

Latest development version of the `atomminer-cli` can be installed using helper setup script:
```bash
wget -qO- https://static.atomminer.com/software/dev/install-amcli.sh | bash
```
or if you need a debug version:
```bash
/bin/bash -c "$(wget -qO- https://static.atomminer.com/software/dev/install-amcli.sh)" 'install' --debug
```

Install script does support "dry-run" mode when only update plan is printed and no actual changes are made to the file system:
```bash
/bin/bash -c "$(wget -qO- https://static.atomminer.com/software/dev/install-amcli.sh)" 'install' --dry
```

This script will backup your current version of `atomminer-cli`, then check your current CPU architecture and download correct new binary version for your architecture. NOTE: this script designed to work with Debian-based Linux distributions only. If you're using anything other than Debian, please download files manually.

Installation example:
```bash
pi@raspberrypi:~ $ wget -qO- https://static.atomminer.com/software/dev/get-am-cli.sh | bash
Backing up atomminer-cli to atomminer-cli.backup.1639406728511644757...
Downloading new version for armhf architecture...
--2021-12-13 18:45:29--  https://static.atomminer.com/software/dev/armhf/atomminer-cli
Connecting to static.atomminer.com (static.atomminer.com)|104.21.83.83|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2023720 (1.9M)
Saving to: ‘/var/atomminer/atomminer-cli’

/var/atomminer/atomminer-cli    100%[============================>]   1.93M  1.95MB/s    in 1.0s    

2021-12-13 18:45:30 (1.95 MB/s) - ‘/var/atomminer/atomminer-cli’ saved [2023720/2023720]

Install complete. atomminer-cli is now based on latest git commit 6325b06199ccad28543f07f17be5d45b36164497

Please restart your atomminer-cli to use updated version
pi@raspberrypi:~ $ /var/atomminer/atomminer-cli
AtomMiner CLI Miner 1.0.3RC9 
  -- built on Dec 13 2021 15:20:57 with arm GCC 5.4.0 20160609
	 Git Revision: 6325b06199ccad28543f07f17be5d45b36164497 

AtomMiner CLI v.1.0.3RC9 is already running with PID 443.
   Status :  Running  with PID: 443 for 12 days 288:11:12
   API srv:  Off 
   Cloud  :  Off 
```

### Notes on Debug Builds

If you decided to run a debug build on your system, please don't forget to clean up your `/var/atomminer/log/` folder every once in a while. The best practice would be to clean it up right before you start or restart `atomminer-cli`.

Please join our Discord or Telegram if you have questions or want to report issues and/or debug logs. 