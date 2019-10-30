---
layout: post
title: AHA-Scraper Basics
date:   2019-10-24 00:00
description: Overview of AHA-Scraper features and basic usage.
toc: true
tags: AHA-Scraper Basics
---

## Running on Windows

### Dependencies

The Windows AHA-Scraper has no real dependencies, everything you need is included in the repo. If you want to gather information about open pipes (optional), you will need to download "handle" from the microsoft sysinternals suite and put it in the "handle" directory located within the "deps" directory.

### Usage (run from AHA-GUI) ***NEW***

As of v0.7.0b1 and higher, AHA-GUI now supports directly running the scraper on Windows (other platforms coming in a future release). If you're just interested in trying the whole suite of tools out together anyway, we encourage you to try this. Go to the [AHA-GUI Releases Page](https://github.com/aha-project/AHA-GUI/releases) and get the current release of AHA-GUI which includes the Windows AHA-Scraper. To run the scraper, start AHA-GUI, then go to `File -> Run AHA-Scraper...`. For more information on this feature as well as how to use AHA-GUI please refer to the AHA-GUI Basics guide.


### Usage (standalone usage on a machine without AHA-GUI)

use git clone, the github client, or click 'download project' to get the repsitory on your computer. Then cd into the directory and run:

```powershell.exe -File .\AHA-Scraper.ps1```

Upon completion, `BinaryAnalysis.csv` will be produced containing the results of the scan.

#### Command Line Options

By default the script scans for a few seconds, to generate at least some traffic statistics on supported platforms (so you can see which connections had 0 bytes transfered, and which had thousands, and see connections come and go over time). To scan for more than a few seconds use the -SecondsToScan argument to provide a number of seconds to scan. For example:

```powershell.exe -File .\AHA-Scraper.ps1 -SecondsToScan 60```

Example:

![Image](https://aha-project.github.io/images/AHA-Scraper.png)

Note: depending on your host security setup, you may need to Allow unsigned powershell scripts: `Set-ExecutionPolicy RemoteSigned` and hit `y` and enter. At some future date we will look into signing the necessary files to avoid this.

## Running on Linux - This section is outdated and will be updated soon

Intial support for linux distros (presently tested on RHEL/Fedora/CentOS and Ubuntu/Debian) is now available!

### Dependencies
The Linux AHA-Scraper presently has no real dependencies, but is only tested on 64bit versions of RHEL and Debian derivitives (RHEL7/CentOS7/Ubuntu 18.04/Kali Linux (at present) specifically). Support for other distros will evolve and improve as time goes on. As of August 1st 2018 there are no longer any known issues that impede the general usage of the Linux scraper. In the future we will be releasing a python 2.7 version (summer 2019?) followed by a migration to python 3.x in sept/oct.

### Usage
use git clone, the github client, or click 'download project' to get the repsitory on your computer. Then cd into the directory and run:

```./AHA-Scraper-Linux.sh```

Upon completion, `BinaryAnalysis.csv` will be produced containing the results of the scan.

Note: you may need to make the script executable before running by using `chmod +x ./AHA-Scraper-Linux.sh`

