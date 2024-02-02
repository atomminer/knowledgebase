---
title: How to update mining software?
tags: ['am01','update']
excerpt: This article describes steps required to update your mining software (atomminer-cli) to the latest available version
createdAt: 2022-02-09 23:21:00
sticky: true
---

## When to Update Mining Software

Most software updates are not mandatory and users have the freedom to decide if they want to update to the latest version and try new features and/or improvements or if they are happy with their current working version. By default, mining software known as `atomminer-cli` will perform update checks and notify users when new update is available:
```
[2022-02-10 03:21:18] [UPD] Checking for firmware updates...
[2022-02-10 03:21:19] [UPD] Checking for software updates...
[2022-02-10 03:21:20] New software update `1.0.3RC10` available
[2022-02-10 03:21:20] New minor software update `1.0.3RC10-d57a2d35f0e9f309276fbe85a2acd8f846137f3f` available
```
Above log snippet shows that new public release version 1.0.3RC10 is available for download. Minor updates are usually posted as [Ongoing Development and Debug builds](/kb/development-and-debug-software-builds) and can be ignored, unless you want to see what was improved or changed way ahead of the next public release.

### How to update?

#### Linux OS

If your software was installed via `apt-get` or similar package manager, the best way to let it perform the update via following command:
```bash
sudo apt-get update && sudo apt-get install atomminer-cli
```

If you're running Linux distro that doesn't support `dpkg/apt-get` package manager, you can download `tar.gz` archive for your architecture and extract it over your existing installation.

Don't forget to restart `atomminer-cli` after update to make sure you're running the most recent version.

#### MacOS

On MacOS `atomminer-cli` is only available through [brew.sh](https://brew.sh) package manager and can be updated with following command:
```bash
brew install atomminer-cli
```
Don't forget to restart `atomminer-cli` after update to make sure you're running the most recent version.

### How to downgrade?

In some rare cases our users may prefer to use one of the older software versions. Whatever your reasons are, you can downgrade at any time you want. All public releases are published in our [apt archive](https://apt.atomminer.com/archive/) and can be manually installed and downloaded.

Please note that we're not archiving any development or debug builds on our servers. Only public releases are saved and published.

### Installation/Software Issues?

Please contact [AtomMiner Support](mailto:support@atomminer.com) if you have issues installing updates or experiencing a problem running the most recent software with problem description and relevant technical information.

Related: [Reporting Issues and Requesting New Features](/kb/report-issues-and-feature-requests)