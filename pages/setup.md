---
# permalink: /about/
layout: single
title: "C# Basics"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-03-21
---

<!-- ![C# Logo](/fhstp-bcc-csharp/assets/images/c-sharp.png) -->

# C# Environment Setup

Windows/MacOS/Linux: Visual Studio Code + .Net 10 
  
## Install Visual Studio Code and .NET 10 SDKs

  * [Visual Studio Code](https://code.visualstudio.com/)
  * [.Net 10 SDKs (Long Term Support)](https://dotnet.microsoft.com/en-us/download/dotnet)
  * [All .NET 10.0 downloads](https://dotnet.microsoft.com/en-us/download/dotnet/10.0)
  * What is SDK? A Software Development Kit (SDK) is a set of tools, libraries, documentation, and code samples that developers use to create applications for a specific platform, operating system, or programming language. 

### Install Visual Studio Code Extension

  <!-- * [.NET MAUI (Optional)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-maui): official Microsoft extension for MAUI developers. -->
  <!-- * The following two extensions should come automatically. -->
  * [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)
      * Using C# Dev Kit requires you to sign in with a Microsoft account that has an active Visual Studio subscription. Visual Studio Community, for example. You can sign in with your own account or ustp account.
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  * [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): Extension for Markdown documents.
  * [CSharp to PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml): Extension for converting Calss relationship in C# to PUML format. 
  * [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml): Extension for visualizing PUML format in an image.
  
### Additional Settings (Optional)
  * Turn on Word Wrap in the Setting if preferred.

## CSharp to PlantUML via Extension of VSCode
  * Prerequisite is needed (check the documentation of the extension): 
    * Windows/Mac OS/Linux: [.NET Core 8.0 Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/8.0/runtime) -> Choose "Run console apps" to download
  * If you want more information in your diagram, you can do certain settings. For example,
    * CSharp to PlantUML extension -> settings -> Create Association -> Create object association from field and property reference 
  * Usage: Ctrl+Shift+P to enable the vscode Command Palette and run the command "csharp2plantuml.classDiagram".
   
<!-- ## CSharp to PlantUML (Optional, in case you don't find the extension through Visual Studio Code)

  * [CSharp to PlantUML](https://github.com/pierre3/PlantUmlClassDiagramGenerator)
  * Install
  ```bash
  $ dotnet tool install --global PlantUmlClassDiagramGenerator
  ```
  * Use
  ```bash
  $ puml-gen ./Program.cs -public
  ```
  * Copy the text from *.puml and visualize using [PUML Viewer](https://www.planttext.com) -->

## PlantUML via Extension of VSCode
  * PlantUML is an Extension for viewing *.puml files.
  * Prerequisite maybe needed (check the documentation of the extension): 
    * Windows: [Java runtime](https://www.java.com/en/download/)
    * Mac OS: [Java runtime](https://www.java.com/en/download/) + graphviz 
      * Note that if you changed the default path of your package manager (i.e., homebrew), you will need to specify it through the "plantuml.commandArgs" command.
  * Usage: Alt+D (Windows)/Option+D (Mac OS) to enable the preview function

## Markdown All in One via Extension
  * Usage: Ctrl + Shift + V or Ctrl K+V to preview the file

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
namespace CRC_CSD_01;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

In CRC_CSD-00.csproj [Doc](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file)
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

In CRC_CSD-00.csproj
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

namespace CRC_CSD_01; // Application namespace (defined by the programmer)

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

namespace CRC_CSD_01; // File scoped namespaces

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
  * [JetBrain Ridar](https://www.jetbrains.com/rider/)
* [Microsoft Learn](https://learn.microsoft.com/en-us/)
* [Debugging in VS Code](https://code.visualstudio.com/docs/csharp/debugging)
<!-- * [Announcing the .NET MAUI extension for Visual Studio Code](https://devblogs.microsoft.com/visualstudio/announcing-the-dotnet-maui-extension-for-visual-studio-code/)  -->
* [Creating C4 and UML Diagrams Using PlantUML with VSCode Extension](https://medium.com/@robertdennyson/creating-c4-and-uml-diagrams-using-plantuml-with-vscode-extension-90032a21ec43)
* [File Scoped Namespaces](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-10.0/file-scoped-namespaces)
* [User and workspace settings](https://code.visualstudio.com/docs/configure/settings)