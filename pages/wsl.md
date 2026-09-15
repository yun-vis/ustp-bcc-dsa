---
# permalink: /about/
layout: single
title: "VSCode WSL Setup"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-09-15
---


The core components of the Windows Subsystem for Linux (WSL) are included by default in Windows 11, but you still need to enable the feature and install a Linux distribution before you can use it. [1] (https://learn.microsoft.com/en-us/windows/wsl/setup/environment), [2] (https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux)

## Install/Uninstall a Unix/Linux distribution

*  WSL stands for Windows Subsystem for Linux
*  Install Ubuntu-24.04 for now as Ubuntu-26.04 has other Windows Defender issues.

```bash
// Check the list of all available distros and versions
$ wsl --list --online
// Install a distribution
$ wsl --install Ubuntu-24.04
// List your installed distributions 
$ wsl -l -v
// Uninstall a distribution when it is not necessary anymore
// $ wsl --unregister Ubuntu-24.04
```

## Install .NET sdk

```bash
$ sudo apt update
$ sudo apt upgrade
// Install .NET sdk
$ sudo apt install dotnet-sdk-10.0
```

## Install Modern Roslyn-Based Tool (Modern, Recommended)
```bash
$ dotnet tool install --global mcdg
// Under the project root folder
// create a subfolder called docs
$ mkdir docs
// create diagram and store it under docs/
$ mcdg -p ./MyProject -o ./docs/diagram.md
```

## PlantUml Class Diagram Generator (Enterprise Standard)

### Step1: Convert Project Folder to PlantUML (Install .NET Global Tool) 
```bash
# // Necessary for the tool puml-gen
# $ sudo apt install dotnet-sdk-8.0
# // $ sudo apt remove --purge dotnet-sdk-8.0
$ dotnet tool install --global PlantUmlClassDiagramGenerator
$ puml-gen ./MyProject ./diagrams/full.puml -dir -allInOne -excludePaths bin,obj,Properties -createAssociation
```

### Step2: Preview PlantUML [Check the setup page](https://yun-vis.net/ustp-bcc-dsa/pages/setup)

* Current Issues
  * After install wsl Ubuntu-26.04, puml-gen still cannot be run due to the windows defender issue. Need to monitor this next year.


