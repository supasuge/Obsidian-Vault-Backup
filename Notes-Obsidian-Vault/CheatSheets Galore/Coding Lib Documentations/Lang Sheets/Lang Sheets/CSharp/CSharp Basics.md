## Table of Contents

- [Hello World in C\](#Hello\World\in\C\)
        - [Keywords](#Keywords)
    - [Hello World in C++](#Hello\World\in\C++)
      - [Hello World In Java](#Hello\World\In\Java)
    - [What is C\# Used for?](#What\is\C\#\Used\for?)
    - [.NET](#.NET)

### Hello World in C\#
##### Keywords
```bash
JIT - Just In Time (Compiler)
CLR - Common Language Runtime

```




```C#
using System;
class Program
{
    static void Main(string[] args)
    {
	    Console.WriteLine("Hello, World!");
    }
}
```

### Hello World in C++
```cpp
#include <iostream>
int main() {
    std::cout << "Hello World";
	return 0;
}
```

#### Hello World In Java
```java
public class Main
{
	public static void main(String[] args)
	{
		System.out.println("hello World");
	}
}
```
- Commenced in the late 1990's. Known initially as `Cool` an acronym for "C-like Object Oriented Language"
- The driving force for the project was to build a language that offered power such that of C++ combined with the simplicity of Visual Basic

`C#` Is one ofer *Several* languages that can be used to build .NET applications but by far the most dominant
- Others:
	- F#
	- Visual basic

`.NET` Framework is language agnostic software development and runtime platform developed by microsoft. It provides a controlled environment for developing and running applications. 

	Progams written for the .NET framework execute in a software environemnt known as the `Common Language Runtime` (CLR) an application Virtual machine that provides services such as security, memory management, and exception handling. There are many dif components to the .NET framework

- **Common Langauge Runtime**: the execution egnine for the .NET framework applications. Provides services such as memory management and threading etc.c
- .NET Framework Class Library is a standard library that encapsulates many common functions, such as file reading and writing, graphic rendering, database interaction, and XML Document manipulation
- Common Language Specification (CLS) is a set of rules and standards that enforce language interoperability.
Common Type System (CTS) is a standard that defines all possible data types and programming constructs supported by CLR and how they interact.

A `Statically compiled` language compiles the source code to machien code before execution. 

a `JIT`


### What is C\# Used for?
- Console APplications:
	- CLI, GUI, simple scripts
- Windows Forms Applications (WinForms): GUI desktop applications packed with arich set oof controls
- Windows presentation foundation (WPF) application: WPF Offeres a framework for creating sopiphisticated desktop applications with advance UI features sucas graphics, multimedia animations etc.
- `Universal Windows Platform Ap`: UWP Apps are designed to provide a universal experience across windows 10, windows 10 mobilee.....
- 

### .NET

There are a few ways to install `.NET`. Regardless of how or what platform you install `.NET` onto, you can validate your installation by running `dotnet --version` from a terminal window.

```powershell-session
C:\> dotnet --version

7.0.304
```

This command will output the version of .NET installed on your machine. If you see the version number of the version you installed, then the installation was successful.

|Operating System|Installation|
|:--|:--|
|Windows|The easiest method to install `.NET` onto Windows is via the `winget` package manager. You can refer to the [Microsoft installation documentation for Windows](https://learn.microsoft.com/en-us/dotnet/core/install/linux) for other installation methods.|
|Linux|Most Linux distributions provide official versions of `.NET`. Check your package manager for install instructions or refer to the [Microsoft installation documentation for Linux](https://learn.microsoft.com/en-us/dotnet/core/install/linux) for other installation methods.|
|macOS|You can either install via the installer downloading from the `.NET Website` or install via `homebrew`. Refer to the [Microsoft installation documentation for macOS](https://learn.microsoft.com/en-us/dotnet/core/install/macos)|
