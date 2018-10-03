# About AHA

AHA is a suite of tools, presently this is two halves, the scraper component available for [Linux](https://github.com/aha-project/AHA-Scraper-Lin) or [Windows](https://github.com/aha-project/AHA-Scraper-Win) which gathers the information, and [AHA-GUI](https://github.com/aha-project/AHA-GUI) visualizer which displays the results of the scraper.

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

Note: depending on your host security setup, you may need to Allow unsigned powershell scripts: `Set-ExecutionPolicy RemoteSigned` and hit `y` and enter. At some future date we will look into signing the necessary files to avoid this.

## Running on Linux

Intial support for linux distros (presently tested on RHEL/Fedora/CentOS and Ubuntu/Debian) is now available!

### Dependencies
The Linux AHA-Scraper has no real dependencies, but presently is only tested on 64bit versions of RHEL and Debian derivitives (RHEL7/CentOS7/Ubuntu 18.04/Kali Linux (at present) specifically). Support for other distros will evolve and improve as time goes on. As of August 1st 2018 there are no longer any known issues that impede the general usage of the Linux scraper.

### Usage
use git clone, the github client, or click 'download project' to get the repsitory on your computer. Then cd into the directory and run:

```./AHA-Scraper-Linux.sh```

Upon completion, `BinaryAnalysis.csv` will be produced containing the results of the scan.

Note: you may need to make the script executable before running by using `chmod +x ./AHA-Scraper-Linux.sh`

# Using AHA-GUI

## Dependencies and Installation

AHA-GUI requires the latest Java 1.8.0 (Java 10 has not been tested yet, but will be validated in the near future).

Get the current built and zipped version from our [Releases Page](https://github.com/aha-project/AHA-GUI/releases) and unzip it in a place of your choosing.

Note: If you would like to build from source there are additional requirements and instructions, please refer to the readme in the AHA-GUI repository for more info.

## Running AHA-GUI

1. Move or copy the `BinaryAnalysis.csv` output from AHA-Scraper into the directory that contains AHA-GUI.jar
1. Either double click `AHA-GUI.jar` or open a terminal/powershell and cd to the correct directory and type `java -jar AHA-GUI.jar`

If you invoke from the commandline, the following commandline arguments can be used:
 * --debug : print additional information to console while running
 * --single : use single lines between nodes with multiple connections
 * --bigfont : use 18pt font instead of the default 12pt font (good for demos)
 * scorefile=scorefile.csv : use the scorefile specified after the equals sign
 * inputFile=inputFile.csv : use the inputFile specified after the equals sign
 * lowVulnThreshold=25 : use the integer after the equals as the minimum node score to get a low vulnerability score (green)
 * medVulnThreshold=15 : use the integer after the equals as the minimum node score to get a medium vulnerability score (yellow)

Example of AHA-GUI running:

![Image](https://aha-project.github.io/images/AHA-GUI.png)

## AHA-GUI Main Window Walkthrough

In the above image we can see the gui as it appears at launch. The lefthand side is the main AHA-GUI window, the righthand side is the Graph Node Inspector. The Graph Node Inspector is shown by default at launch, and shows details of the graph nodes when they are clicked. There are two checkboxes at the bottom which provide inspector options. One is an option to update the inspector when you hover on nodes. There is also an option to show metric specifics, which will show exactly which criteria matched (you must click/hover on a new node after enabling this option for it to take effect).

The main GUI is largely comprised of the graph view which shows external conenctions in red, internal connections in white, and duplicate connections in darker versions of each of those colors (i.e. darker red, and gray for duplicate connections). Connections in TIME_WAIT, CLOSE_WAIT, SYN_SENT, etc. are drawn as a dashed line. All solid connections with the exception of the connections drawn to the "External" virtual node are established connections. 

The "External" virtual node is drawn to help visualize any service which has a port bound that would allow connection from an external entity. This node can be hidden by selecting "Hide Ext Node" from the bottom area of the main graph view.

Along the bottom of the main AHA-GUI window there are two areas. The bottom-most area contains buttons and checkboxes to alter view and other options which will be discussed further below. Above that is a textual area which outputs a summary of what is available in the inspector view including the name of the process/graph node, the user and path of the process, connections it has to other processes, and a summary of all the score metrics which matched their criteria. Below this output area is a search bar which allows graph nodes/edges to either be emphasized (highlighted/edged in blue) or hidden (entirely hidden from the graph). Example syntax for the search bar:

`processname==svchost.exe` will emphasize svchost.exe
`~processname==spoolsv.exe` will hide spoolsv.exe
`processname!=svchost.exe` will emphasize everything that is *not* svchost.exe
`~processname==spoolsv.exe` will hide everything that is *not* spoolsv.exe

These terms can also be or'd together with the `||` symbol:

`processname==svchost.exe || ~processname==spoolsv.exe` will emphasize svchost.exe and hide spoolsv.exe at the same time. Please note this syntax is lazily evaluated from beginning to end per token. This means we split the complex query up by `||` and then process starting with the leftmost term, moving rightward. Thus if there is any conflict between the terms, the rightmost will win.


The buttons/checkboxes along the bottom have the following uses (from left to right):
- Show data view: this opens a tabular view which shows results data generated by the GUI. This view will be explained in a later section of this document.
- Reset Zoom: this button will reset the graph scale if things go awry, as they sometimes do.
- Show Inspector: if you have hidden the inspector window by closing it, this will re-summon it.
- Score method dropdown: presently contains 3 items, "Normal", "WorstCommonProc", and "ECScore". Normal uses the "Normal" score method described later which uses metrics from the MetricsTable.config located in the same directory as the AHA-GUI.jar. WorstCommonProc gives all processes with the same username the worst score for any proc under that user. ECScore is the EigenCentrality score method, which is the first attempt at weighting nodes closer to external nodes as having more attack-ability.
- Hide OS Procs: Hides operating system processes which can sometimes clutter the view such as svchost.exe, spoolsv.exe, lsass.exe, and others. This is presently defined by an internal list of application paths, which will probably some day be split out into a config file similar to MetricsTable.cfg
- Hide Ext Node: this hides the virtual "External" node and all connections to it to help unclutter the graph.
- DNS Names: switches Ext_<IP> format names to Ext_<DNS Name>. Example: would switch "Ext_10.0.0.1 to Ext_SomeHost".
- Custom Scorefile: Overlays custom information from a custom scorefile. Default filename is scorefile.csv. There is an example for the custom scorefile in the AHA-GUI repository in the resources directory. You can use custom scorefile filenames by specifying scorefile=<path/to/customscorefile.csv> on the commandline when staring AHA-GUI.
  
## AHA-GUI Data View Walkthrough  

There are presently two tabs in the Data View: Vulnerabilty Metrics, and Listening Processes.

![Image](https://aha-project.github.io/images/AHA-GUI-DataView-VulnMetrics.png)

![Image](https://aha-project.github.io/images/AHA-GUI-DataView-ListeningProcs.png)
  
## Attack Surface Scoring methodology
As noted above in the AHA-GUI walkthrough, there are several scoring methods, but we will start with the most basic. The standard scoring method used in AHA-GUI is called "Normal". This applies rules from the MetricsTable.cfg located within the same directory as AHA-GUI.jar which is editable as a text file, and contains a default ruleset as well as an explanation of ho w to create new rules.

One thing to keep in mind for the scoring methods discussed in this section, is that they apply to the data that is available. In this case the data was gathered via curports, which is like a very advanced version of netstat (linux uses netstat and supporting other scripts), which provides us with processes and their open ports. We then make further passes on this data to determine security features of the executable binaries which are running for each process, as well as another pass to determine effective process priveleges. While this is a very interesting dataset to analize please note that it does not as yet include any other platform specific security feaures such as ACL discovery, or reading of local machine firewall rules. This means that if the local firewall is active, that would impact established connections (i.e. they would probably not be established if that's what the firewall is there to do), but we will still show ports that are bound on externally available IP address/Port combos as externally available. It is up to the user to audit their firewall/ACLs and ensure that the processes we discovered which could be available from external machines/networks/hosts/etc are properly firewalled.

In general, other scoring methods utilize the outputs of the "Normal" scoring algorithm, and are either asigning weight by positionts in the node graph, or attempting to suss out other connections that are not visible via the tcp connection graph, such as connections by user account (WorstProcScore) or increase/reduction of attackability relative to the externally accessible nodes in the graph (ECScore).
