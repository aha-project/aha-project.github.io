# About AHA

AHA is a suite of tools, presently this is two halves, the [AHA-Scraper](https://github.com/aha-project/AHA-Scraper) host scraper component which gathers the information, and [AHA-GUI](https://github.com/aha-project/AHA-GUI) visualizer which displays the results of the scraper.

# Using AHA-Scraper

## Running on Windows

### Dependencies

The Windows AHA-Scraper has no real dependencies, but presently only works on x86_64. 32bit support will be added at some future date.

### Usage

use git clone, the github client, or click 'download project' to get the repsitory on your computer. Then cd into the directory and run:

```powershell.exe -File .\AHA-Scraper.ps1```

Upon completion, `BinaryAnalysis.csv` will be produced containing the results of the scan.

Example:

![Image](https://aha-project.github.io/images/AHA-Scraper.png)

Note: depending on your host security setup, you may need to Allow unsigned powershell scripts: "Set-ExecutionPolicy RemoteSigned" and hit "y" and enter. At some future date we will look into signing the necessary files to avoid this.

## Running on Linux

Support for linux distros (such as RHEL/Fedora/CentOS and Ubuntu/Debian), and possibly other distributions and operating systems will hopefully debut at a future date.

# Using AHA-GUI

## Dependencies and Installation

AHA-GUI requires the latest Java 1.8.0 (Java 10 has not been tested yet, but will be validated in the near future).

Get the current built and zipped version from our [Releases Page](https://github.com/aha-project/AHA-GUI/releases) and unzip it in a place of your choosing.

Note: If you would like to build from source there are additional requirements and instructions, please refer to the readme in the AHA-GUI repository for more info.

## Running AHA-GUI

1. Move or copy the `BinaryAnalysis.csv` output from AHA-Scraper into the directory that contains AHA-GUI.jar
1. Either double click `AHA-GUI.jar` or open a terminal/powershell and cd to the correct directory and type `java -jar AHA-GUI.jar`

Example:

![Image](https://github.com/aha-project/AHA-GUI/raw/master/resources/AHA-GUI-Screenshot.png?raw=true)
