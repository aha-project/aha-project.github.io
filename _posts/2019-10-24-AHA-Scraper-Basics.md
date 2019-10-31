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

[![Image](https://aha-project.github.io/images/AHA-Scraper.png)](https://aha-project.github.io/images/AHA-Scraper.png)

Note: depending on your host security setup, you may need to Allow unsigned powershell scripts: `Set-ExecutionPolicy RemoteSigned` and hit `y` and enter. At some future date we will look into signing the necessary files to avoid this.

## Running on Linux - This section is outdated and will be updated soon

While the scraper has been marked stable, it still has not received a lot of external testing as of yet, but has been tested internally on CentOS 7.5+ (a RHEL derivative), Kali 2018.2+ (a Debian derivative), and at present we have no known issues.

There is no implied waranty, or liability assumed by us if you run this, but there should not be anything that can cause side effects either.

### Scraper usage
Clone or download the repo from github

To run the scraper:
1. Open a shell
1. `cd` to the directory containing the script
1. Run the script by typing `sudo python python_aha.py`
1. Install any packages that the script says are missing.
1. Display the help menu with `sudo python python_aha.py -h`
1. When run, it will first scan then the data is processed. No new data will be collected in the processing phase. 

### Scraper Help Menu
To use a command line argument follow the base command(`sudo python python_aha.py`) 
with any number of the below arguments. Arguments must appear in a space separated 
list. Any arguments requiring additional fields must have them supplied immediately 
after the argument. Consult the `Defaults` and `Normal Behavior` sections to 
understand how the program works without additional arguments. All argument fields are 
optional, just supply an underscore instead of the field.  
- `h` : Display the help menu.  
- `H` : Do not compute executable hashes.  
- `k` : Ignore kernel processes.  
- `e` : Ignore network entries.  
- `n` : Ignore named pipes.  
- `p` : Ignore all processes.  
  - This will limit the named pipe info. A large amount of infor may be missing.  
- `r` : For repeated scan use most recent scan time. Default is first time it was found.  
- `l` {seconds} : Long scan. Time to scan in seconds.  
- `f` {file}    : Output file to write results to. Relative shell's working directory.  
- `d` {level}   : (**DEV**)Debug menu. Requires 1 arguments, debug level {int}.  
- `o` {file}    : (**DEV**)Output recall file. Requires 1 argument, filename.  
- `i` {file}    : (**DEV**)Simulate run from outputted recall. Requires 1 argument, filename.  

#### Examples 
- `sudo python python_aha.py -l 320 -k`: To scan for 320 seconds and not output kernel processes.  
- `sudo python python_aha.py -p`: To scan without processes. Very limited without 
the process information.  
- `sudo python python_aha.py -f _`: Uses the default file `BinaryAnalysis.csv`.  
- `sudo python python_aha.py -f "s p a c e.csv"`: Outputs to a space seperated file.  

## Defaults (With underscore instead of field)  
- `f` outputs to `BinaryAnalysis.csv`  
- `o`/`i` outputs to `debug-out.json`.  
- `l` runs for 2 minutes or `120` seconds.  
- `d` runs as debug level 4.  
- `r` shows first scan it was found in.  


### Normal Behavior 
- There are three scans:
  - Process 
  - Network
  - Named Pipe
- Scans without the `l` argument will run one cycle which is faster than one second.
Although, scans on the edge of a second may show scanned items in different times.  
- Scan over time defaults to detection time being the first time that row was scanned. 

The resulting `BinaryAnalysis.csv` can either be viewed in a text/spreadsheets app (such as Excel) or visualized in the [AHA-GUI](https://github.com/aha-project/AHA-GUI).

***Note***: The program must be run as sudo.


