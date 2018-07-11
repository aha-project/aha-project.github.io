# Using AHA

AHA is a suite of tools, presently this is two halves, the AHA-Scraper host scraper component which gathers the information, and AHA-GUI visualizer which displays the results of the scraper.

# Using AHA-Scraper

## Windows

### Dependencies

The Windows AHA-Scraper has no real dependencies, but presently only works on x86_64. 32bit support will be added at some future date.

### Usage

clone or download the repo, cd into the directory and run:

powershell.exe -File .\AHA-Scraper.ps1

Note: depending on your host security setup, you may need to Allow unsigned powershell scripts: "Set-ExecutionPolicy RemoteSigned" and hit "y" and enter. At some future date we will look into signing the necessary files to avoid this.

# Using AHA-GUI

