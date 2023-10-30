<p align="center">
<img src="https://raw.githubusercontent.com/freerouting/freerouting/master/design/social_preview/freerouting_social_preview_1280x960_v2.png" alt="Freerouting" title="Freerouting" align="center">
</p>
<h1 align="center">Freerouting</h1>
<h5 align="center">Freerouting is an advanced autorouter for all PCB programs that support the standard Specctra or Electra DSN interface.</h5>

<p align="center">
    <a href="https://github.com/freerouting/freerouting/releases"><img src="https://img.shields.io/github/v/release/freerouting/freerouting" alt="Release version" /></a>
    <img src="https://img.shields.io/github/downloads/freerouting/freerouting/v1.9.0/total" alt="Downloads"/>
    <img src="https://img.shields.io/github/downloads/freerouting/freerouting/total" alt="Downloads"/>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/freerouting/freerouting" alt="License"/></a>
</p>

<h3 align="center">:point_right: This project needs JAVA and UI/UX volunteers! Contact @andrasfuchs for details! :point_left:</h3>

<br/>
<br/>

[Installers for Windows and Linux can be downloaded here.](https://github.com/freerouting/freerouting/releases)

## Introduction

This software can be used together with all host PCB design software systems containing a standard Specctra or Electra DSN interface. It imports .DSN files generated by the Specctra interface of the host system and exports .SES Specctra session files.

Although the software can be used for manual routing in 90 degree, 45 degree and free angle modes, it's main focus is on autorouting.

### Getting started

You can run Freerouting as a standalone application.

1) After launching freerouting.jar, a window appears prompting you to select your exported .DSN design file.
![image](https://user-images.githubusercontent.com/910321/167868226-f046da72-357d-44f6-ba0d-ee27d34725c1.png)

2) After opening a design you can start the autorouter with the button in the toolbar on top of the board window.
![image](https://user-images.githubusercontent.com/910321/167868601-1510f75d-73a2-4321-ac03-2dd4a91732eb.png)

3) While autorouter is running you can follow the progress both visually in the board editor and numerically in the footer.
![image](https://user-images.githubusercontent.com/910321/167869140-6101e9c2-d58d-48fd-b245-6a00225df042.png)

4) You are going to have a short summary when it is finished.
![image](https://user-images.githubusercontent.com/910321/167869313-40cfa1c7-d896-40cd-b485-53da0139562a.png)

5) You can now save your routed board as a .SES file in the File / Export Specctra Session File menu.
![image](https://user-images.githubusercontent.com/910321/167869579-fe40c3ff-09ce-4687-9b78-142706cfc342.png)

If you use [KiCad](#additional-steps-for-users-of-kicad), [Autodesk EAGLE](#additional-steps-for-users-of-autodesk-eagle), [Target3001!](#additional-steps-for-users-of-target-3001) or [pcb-rnd](#additional-steps-for-users-of-pcb-rnd), [click here for their integrations](/docs/integrations.md).

## Using the command line arguments

Freerouter was designed as a GUI program, but it also can function as a command line tool. Typically you would have an input file (e.g. Specctra DSN) that you exported from you EDA (e.g. KiCad). If this file has unconnected routes, you would want to wire those with autorouter, and save the result in a format that you can then import back into your EDA.

The following command line arguments are supported by freerouter:

* -de [design input file]: loads up a Specctra .dsn file at startup.
* -di [design input directory]: if the GUI is used, this sets the default folder for the open design dialogs.
* -dr [design rules file]: reads the rules from a previously saved .rules file.
* -do [design output file]: saves a Specctra board (.dsn), a Specctra session file (.ses) or Eagle session script file (.scr) when the routing is finished.
* -mp [number of passes]: sets the upper limit of the number of auto-router passes that will be performed.
* -l [language]: "en" for English, "de" for German, "zh" for Simplified Chinese, otherwise it's the system default. English is set by default for unsupported languages.
* -host [host_name host_version]: sets the name of the host process, if it was run as an external library or plugin.
* -mt [number of threads]: sets thread pool size for route optimization. The default is one less than the number of logical processors in the system. Set it to 0 to disable route optimization.
* -oit [percentage]: stops the route optimizer if the improvement drops below a certain percentage threshold per pass. Default is 0.1%, and `-oit 0` means to continue improving until it is interrupted by the user or it runs out of options to test.
* -us [greedy | global | hybrid]: sets board updating strategy for route optimization: greedy, global optimal or hybrid. The default is greedy. When hybrid is selected, another option "hr" specifies hybrid ratio.
* -hr [m:n]: sets hybrid ratio in the format of #_global_optiomal_passes:#_prioritized_passes. The default is 1:1. It's only effective when hybrid strategy is selected.
* -is [sequential | random | prioritized]: sets item selection strategy for route optimization: sequential, random, prioritized. The default is prioritized. Prioritied strategy selects items based on scores calculated in previous round.
* -inc [net class names, separated by commas]: auto-router ignores the listed net classes, eg. `-inc GND,VCC` will not try to wire components that are either in the "GND" or in the "VCC" net class.
* -im: saves intermediate steps in version-specific binary format. This allows to user to resume the interrupted optimization from the last checkpoint. Turned off by default.
* -dct [seconds]: dialog confirmation timeout. Sets the timeout of the dialogs that start a default action in x seconds. 20 seconds by default.
* -da: disable anonymous analytics.
* -dl: disable logging.
* -help: shows help.

A complete command line looks something like this if your are using PowerShell on Windows:

```powershell
java.exe -jar freerouting-1.9.0.jar -de MyBoard.dsn -do MyBoard.ses -mp 100 -dr MyBoard.rules
```

This would read the _MyBoard.dsn_ file, do the auto-routing with the parameters defined in _MyBoard.rules_ for the maximum of 100 passes, and then save the result into the _MyBoard.ses_ file.


## Running Freerouting using Java JRE

There are only installers for Windows x64 and Linux x64. Fortunatelly though the platform independent .JAR files can be run on the other systems, if the matching Java runtime is installed.

You will need the following steps to make it work:
1. Get the current JAR release from our [Releases page](https://github.com/freerouting/freerouting/releases)
2. Install [Java JRE](https://adoptium.net/temurin/releases/)
    * Select your operating system and architecture
    * Select `JRE` as package type
    * Select `17` as version
4. Run the downloaded JAR file using the installed java

```powershell
java -jar freerouting-1.9.0.jar
```

(macOS: please note that you can't start Freerouting from the Mac Finder, you must use the Mac Terminal instead!)


## Contributing

We ❤️ all our contributors; this project wouldn’t be what it is without you!

If you want to help out, please consider replying to [issues](https://github.com/freerouting/freerouting/issues), creating new ones, or even send your fixes and improvements as [pull requests](https://github.com/freerouting/freerouting/pulls). [Our developer documention](/docs/developer.md) can help you with the technicalities.
