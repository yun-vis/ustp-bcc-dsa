---
# permalink: /about/
layout: single
title: "C# Environment Setup"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-09-15
---

# Teaching and Learning Environment

Windows/MacOS/Linux: Visual Studio Code + .Net 10 
  
## Install Visual Studio Code and .NET 10 SDKs

  * [Visual Studio Code](https://code.visualstudio.com/)
  * [.Net 10 SDKs (Long Term Support)](https://dotnet.microsoft.com/en-us/download/dotnet)
  * [All .NET 10.0 downloads](https://dotnet.microsoft.com/en-us/download/dotnet/10.0)
    * Latest version: SDK 10.0.401 
  * What is SDK? A Software Development Kit (SDK) is a set of tools, libraries, documentation, and code samples that developers use to create applications for a specific platform, operating system, or programming language. 

### Install Visual Studio Code Extension

  * [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)
      * Using C# Dev Kit requires you to sign in with a Microsoft account that has an active Visual Studio subscription. Visual Studio Community, for example. You can sign in with your own account or ustp account.
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  * [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): Extension for Markdown documents.
    * Usage: Ctrl + Shift + V or Ctrl K+V to preview the file

### Additional Settings (Optional)
  * Turn on Word Wrap in the Setting if preferred. File -> Preferences -> Setting
  * Enable Auto Save. File -> Auto Save

# Class Diagram Generator
  * This tool converts your C# project into a format that can be visualized.

## Option 1: PlantUml Class Diagram Generator (Enterprise Standard)

### Step1-1: Convert Project Folder to PlantUML (Install .NET Global Tool) 
```bash
$ dotnet tool install --global PlantUmlClassDiagramGenerator
$ puml-gen ./MyProject ./diagrams/full.puml -dir -allInOne -excludePaths bin,obj,Properties -createAssociation
```

* **-dir**: Tells the tool that your input and output targets are directories (folders) rather than individual files.
* **-allInOne**: Combines all generated class structures into a single, unified .puml file instead of creating a separate .puml file for every single C# class.
* **-excludePaths bin,obj,Properties**:Tells the generator to completely ignore and skip the specified folders during the scan.
* **-createAssociation**:Automatically draws relationship arrows (associations and aggregations) between classes based on their properties and fields.

### Step1-2: Convert Project Folder to PlantUML (Install Visual Studio Code Extension) 

  * [CSharp to PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml): Extension for converting Calss relationship in C# to PUML format. 
    * In %USERPROFILE%\.vscode\extensions\pierre3.csharp-to-plantuml-X.X.X\out\extension.js,
    Replace
    ```bash
      if ((0, util_1.isUndefined)(wsroot)) 
    ```
    with
    ```bash
      if (wsroot===undefined) 
    ```
  
### Step2: Preview PlantUML (Install Visual Studio Code Extension)
   
<!-- ### PlantUML via Extension of VSCode -->
  * Install extension [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml): Extension for visualizing PUML format in an image.
  * Prerequisite is needed (check the documentation of the extension): 
    * Windows/Mac OS/Linux: [.NET Core 8.0 Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/8.0/runtime) -> Choose "Run console apps" to download
  * If you want more information in your diagram, you can do certain settings. For example,
    * CSharp to PlantUML extension -> settings -> Create Association -> Create object association from field and property reference 
  * Usage: Ctrl+Shift+P to enable the vscode Command Palette and run the command "csharp2plantuml.classDiagram".
  * PlantUML is an Extension for viewing *.puml files.
  * Prerequisite maybe needed (check the documentation of the extension): 
    * Windows: [Java runtime](https://www.java.com/en/download/)
    * Mac OS: [Java runtime](https://www.java.com/en/download/) + graphviz 
      * Note that if you changed the default path of your package manager (i.e., homebrew), you will need to specify it through the "plantuml.commandArgs" command.
  * Usage: Alt+D (Windows)/Option+D (Mac OS) to enable the preview function

## Option 2: Modern Roslyn-Based Tool (Modern, Recommended)

* Mermaid.js is an open-source, JavaScript-based tool that uses simple text and code to generate diagrams and charts dynamically.
* [Mermaid Class Diagram Generator (mcdg)](https://www.nuget.org/packages/mcdg#readme-body-tab)
* The drawback of this tool is that it is blocked by the Windows Defender of our IT. You need to run it on [WSL](https://yun-vis.net/ustp-bcc-dsa/pages/wsl) if you use Lab PCs at the university.

```bash
// Install the open-source Mermaid Class Diagram Generator
$ dotnet tool install --global mcdg
// Under the project root folder
// create a subfolder called docs
$ mkdir docs
// create diagram and store it under docs/
$ mcdg -p ./MyProject -o ./docs/diagram.md
```

* **-p**: Project folder path
* **-o**: Output path

### Potential Errors
  * For Mac users, if your mac cannot recognize mcdg as a command after a full reboot, or 
  ```bash
  $  source ~/.zshrc
  ```
  you will need to add the binary to the .zshrc, so your OS recognizes it.
  ```bash
  echo 'export PATH="$PATH:$HOME/.dotnet/tools"' >> ~/.zshrc && source ~/.zshrc
  ```
  * If you got an error saying that you are missing Framework: 'Microsoft.NETCore.App', version '8.0.0' (x64), reinstall mcdg by add [**--allow-roll-forward** parameter](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-tool-install#options). This parameter allows the tool to use a newer version of the .NET runtime if the runtime it targets isn't installed.
  ```bash
  $ dotnet tool uninstall -g mcdg
  $ dotnet tool install -g mcdg --allow-roll-forward
  ```

## Check Your Installed .Net SDK

```bash
// Display help
$ dotnet -h|--help
// check the installed .Net in your system
$ dotnet --info 
// check the current .Net Version used for command line
$ dotnet --version
// check all installed SDKs
$ dotnet --list-sdks
// If you want to change your .Net Version, add the globaljson file
// The global.json file allows you to define which .NET SDK version 
// is used when you run .NET CLI commands.
// Uses the highest installed feature band and patch level that matches 
// the requested major and minor with a feature band and patch level that is 
// greater than or equal to the specified value. If not found, fails.
// --roll-forward latestFeature: Configures the SDK to use the highest installed feature band and patch level for the specified major/minor version.
$ dotnet new globaljson --sdk-version 8.0.302 --roll-forward latestFeature
# $ dotnet new globaljson --force --sdk-version 8.0.302 --roll-forward latestFeature
// 8.0.302 follows a specific Major.Minor.Patch structure
// 8 (Major): Represents the major .NET release (e.g., .NET 8).
// 0 (Minor): Aligns with the minor version of the .NET runtime.
// 100 (SDK Patch): This 3-digit number is split into: 1 (Feature Band) + 00 (Patch Level)
// Check for update
$ dotnet sdk check
```

More about globaljson can be found [here](https://learn.microsoft.com/en-us/dotnet/core/tools/global-json).


## First Console Program

```bash
// Create a new console project with a specific project name
$ dotnet new console --name MyProject
// Use top-level statements
$ dotnet new console 
// Skip top-level statements and include Main()
$ dotnet new console --use-program-main 
```
or call .NET New Project (Ctrl+Shift+P) in the [VSCode Command Palette](https://code.visualstudio.com/docs/getstarted/userinterface#:~:text=The%20most%20important%20key%20combination,for%20the%20most%20common%20operations.)

In Program.cs [Doc](https://aka.ms/new-console-template)
### Use top-level statements
```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

### Skip top-level statements and include Main
```csharp
namespace MyProject;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

In MyProject.csproj [Doc](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file)
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

In MyProject.csproj
```bash
// You build your project against APIs defined in a target framework moniker (TFM). You specify the target framework in the project file.
  <TargetFramework>net9.0</TargetFramework>
// or multiple target frameworks. Note that the element name is now plural.
  <TargetFrameworks>net10.0;net8.0;net47</TargetFrameworks>
```

## How to Run the Program

```bash
// Build the program
$ dotnet build
// Run the program (Including Build, so you can skip the above command) 
$ dotnet run
```

## Namespace in C#
```csharp
using System;         // System namespace (defined by C#)
                      // The "using" Directive
// or
// using static System.Console;  // Console is a static system class
/*
A static class is basically the same as a non-static class,
but there is one difference: a static class cannot be instantiated.
*/

namespace MyProject; // Application namespace (defined by the programmer)

class Program
{
    // static: shared method of all instances by the class
    // void: return "nothing" in the method
    // string[] args: parameters passed to the main function.
    // The parameters can be taken when lauching the application.
    static void Main(string[] args)  // Where the application begins
    {
        Console.WriteLine("Hello, World!");   // Console is a system class
    }
}
```

## Console Program with Accompanying Parameters
```csharp
using System;

namespace MyProject; // File scoped namespaces

class Program
{
    // string[] args: parameters passed to the main function.
    static void Main(string[] args) // Where the application begins
    {
        Console.WriteLine("The length of the arguments: " + args.Length);
        for( int i = 0; i < args.Length; i++ ){
            Console.WriteLine(args[i]);
        }
    }
}
```

## Introduction to Markdown language
  * [Markdown Official Website](https://daringfireball.net/projects/markdown/)
  * [Markdown Guide](https://www.markdownguide.org/)


## Example for a README.md file in your Project

  * [Best README Template](https://github.com/othneildrew/Best-README-Template)

  * About The Project
    * Built With
  * Getting Started: How to install and set up your app.
    * Prerequisites
    * Installation
  * Usage: Show useful examples of how a project can be used.
  * Roadmap: What have been implemented and what are the planed features.
  * Contributing: Encourage people to work on your project.
  * License: Your project license
  * Contact
  * Acknowledgments

## Uninstall .NET SDKs

* Windows does not have a native dotnet uninstall terminal command, but you can easily uninstall it through setting Apps/Installed apps. For more information, check [here](https://learn.microsoft.com/en-us/dotnet/core/install/remove-runtime-sdk-versions?pivots=os-windows).

# Potential Errors

* Explorer in VSCode is missing. Ctrl+Shift+P to enable the vscode Command Palette and run the command "View: Reset View Locations".

* On Mac OS, if the default Debugger not running on VS Code, make sure
  * Your C# and C# Dev Kit are up-to-date
  * Setup workspace settings
    * Put [launch.json](/ustp-bcc-csharp/assets/files/launch.json) and [tasks.json](/ustp-bcc-csharp/assets/files/tasks.json) under .vscode of your root folder (Thanks Timon Schneider for the solution)

---
# External Resources

* Additional IDEs
  * [Visual Studio](https://visualstudio.microsoft.com/vs/)
    * [Install Visual Studio](https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022)
  * [JetBrain Rider](https://www.jetbrains.com/rider/)
* [Microsoft Learn](https://learn.microsoft.com/en-us/)
* [Debugging in VS Code](https://code.visualstudio.com/docs/csharp/debugging)
<!-- * [Announcing the .NET MAUI extension for Visual Studio Code](https://devblogs.microsoft.com/visualstudio/announcing-the-dotnet-maui-extension-for-visual-studio-code/)  -->
* [Creating C4 and UML Diagrams Using PlantUML with VSCode Extension](https://medium.com/@robertdennyson/creating-c4-and-uml-diagrams-using-plantuml-with-vscode-extension-90032a21ec43)
* [File Scoped Namespaces](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-10.0/file-scoped-namespaces)
* [User and workspace settings](https://code.visualstudio.com/docs/configure/settings)
* [The .NET Compiler Platform SDK (also called the Roslyn APIs)](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/)
 