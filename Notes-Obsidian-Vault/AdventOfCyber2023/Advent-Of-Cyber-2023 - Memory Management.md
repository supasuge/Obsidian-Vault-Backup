## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Task](#Task)
- [Log Analysis](#log\analysis)
          - [Objectives](#Objectives)
    - [What exactly is a Proxy?](#What\exactly\is\a\Proxy?)
- [FTK Imager Task](#ftk\imager\task)
          - [Task](#Task)
    - [Working with FTK Imager](#Working\with\FTK\Imager)
      - [User Interface](#User\Interface)
      - [Previewing Modes](#Previewing\Modes)
- [C# Shells and C2's](#c#\shells\and\c2's)
          - [Objectives](#Objectives)
    - [Handling 101](#Handling\101)
    - [**Namespaces, classes, functions and variables**](#**Namespaces,\classes,\functions\and\variables**)
    - [C# Syntax table and explainations](#C#\Syntax\table\and\explainations)

## Table of Contents

    - [Task](#Task)
- [Log Analysis](#log\analysis)
          - [Objectives](#Objectives)
    - [What exactly is a Proxy?](#What\exactly\is\a\Proxy?)
- [FTK Imager Task](#ftk\imager\task)
          - [Task](#Task)
    - [Working with FTK Imager](#Working\with\FTK\Imager)
      - [User Interface](#User\Interface)
      - [Previewing Modes](#Previewing\Modes)
- [C# Shells and C2's](#c#\shells\and\c2's)
          - [Objectives](#Objectives)
    - [Handling 101](#Handling\101)
    - [**Namespaces, classes, functions and variables**](#**Namespaces,\classes,\functions\and\variables**)
    - [C# Syntax table and explainations](#C#\Syntax\table\and\explainations)

- When strings are written to memory, each character is written in order.
	taking 1 byte each. A NULL character is simply a byte with value 0x00, which can be seen by changing the debug panel to hex mode.
- ==C++== doesn't check variable boundaries, it reads your name from the start of the `player_name` variable to the first NULL byte it finds. 
- In C++, memory is stored in a very particular way; I.e., `little-endian` byte order.
	- 

### Task
- Download attempt of a malicious binary - connection to a know malicious URL binary.
- Data Ex-filtration - High count of outbound bandwidth due to file upload



1. Get 16 coins.
2. Rename yourself to `AAAABBBBCCCCDDDD`

# Log Analysis
###### Objectives
- Understanding Proxies
- Proxy based log based on typical use cases

| Field | IP | Description |
|:-:|:-:|:-:|
|**Source IP Address**|158.32.51.188|The source (computer) that initiated the HTTP request.|
| **Timestamp** | [25/Oct/2023:09:11:14 +0000] |  The date and time when the event occurred.
| **HTTP Request** | GET /robots.txt HTTP/1.1 | The actual HTTP Request made, including the request method, URI path, and HTTP version. |
| **Status Code** | `200` | The actual HTTP request made, including the request method, URI path, and HTTP version |
| **User Agent** | curl/7.68.0 | The user agent  used by the source of the request. It is typically tied up to the application it is associated with | 

![[Pasted image 20231209211343.png]]
### What exactly is a Proxy?
- Server that is an intermediary between two devices, and the internet. When you request information, it's first routed through the proxy which () before the request is sent to the destination.

| **Attack Technique** | **Potential Indicator** |
|:-:|:-:|
|**Download attempt of a malicious binary**|Connection to a known malicious URL binary<br><br>(e.g. www[.]evil[.]com/malicious[.]exe)|
|**Data exfiltration**|High count of outbound bandwidth due to file upload<br><br>(e.g. outbound connection to OneDrive)|
|**Continuous C2 connection**|High count of outbound connections to a single domain in regular intervals<br><br>(e.g. connections every five minutes to a single domain)|
#proxy #adventofcyber2023
#tail
`tail` - allows you to view the end of the file easily
```bash
tail -n 1 access.log
```

#wordcount #numberlines
`wc` - Stands for `word count`... Pretty self explainatory
`nl` - Stands for `number lines`... Also rather self explainatory


|**Position**|**Field**|**Value**|
|:-:|:-:|:-:|
|1|Timestamp|[2023/10/25:16:17:14]|
|2|Source IP|10.10.140.96|
|3|Domain and Port|storage.live.com:443|
|4|HTTP Method|GET|
|5|HTTP URI|/|
|6|Status Code|400|
|7|Response Size|630|
|8|User Agent|"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36  <br>(KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36"|

#cut
`cut` - allows you to extract specific sections (columns) of lines from a file or input stream by "cutting" the line into Columns based on a *delimiter* and selecting which columns to display. `-d` and the `-f` 

This example uses `(' ')` as its delimiter and only displays the timestamp after cutting the log with space.
```bash
cut -d ' ' -f1 access.log
[2023/10/25:15:42:02]
[2023/10/25:15:42:02]
--- REDACTED FOR BREVITY ---
```

- Possible to select multiple columns, just like below (chooses the  timestamp (*column #1*))
```bash
cut -d ' ' -f1,3,6 access.log
```
`sort` - sort lines of text files/input streams in ascending/descending order.

`uniq` - Allows you to filter out and display unique lines from a sorted file or input stream
	Flags: `-c` option to the `uniq` command.  

```shell
cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort -n
     78 partnerservices.getmicrosoftkey.com
    113 **REDACTED**
    118 ocsp.digicert.com
    123 officeclient.microsoft.com
--- REDACTED FOR BREVITY ---
```

Based on the result, you can see that the count of connections made for each domain is sorted in ascending order. If you want to make the output appear in descending order,  use the `-r` option. Note that it can also be combined with the `-n` option (`-nr` if written together).

```bash
cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort -nr
```


Q: **How many unique IP addresses are connected to the proxy server?**
```bash
cut -d ' ' -f2 access.log | sort | uniq -c | wc
```
`9`

Q: **How many unique domains were accessed by all workstations?**
```bash
cut -d ' ' -f2,3 access.log | grep thm | sort | uniq -c
```

![[Pasted image 20231209230035.png]]



____



# FTK Imager Task

- Use FTK imager to track down and piece together `McGreedy`'s deleted digital breadcrumbs
	- Analyse Digital Artefacts and evidence
	- Recover deleted digital artefacts and evidence
	- Verify the integrity of a drive/image used as evidence

###### Task
- *Van Sprinkles* a disgruntled employee scatters USB drives loaded with malware. Little do the *AntarctiCrafts* 
> open `FTK` Imager and navigate to `File > Add Evidenve Item` `Physical Drive` 
> then choose "\\PHYSICALDRIVE2 - Microsoft Virtual Disk[1GB SCSI]"

### Working with FTK Imager

![[Pasted image 20231209235935.png]]

![[Pasted image 20231209235943.png]]

#### User Interface
- `"x"` icon is for deleted files
1. *Evidence Tree Pane*
2. *File List Pane*
3. *Viewer Pane*

![[Pasted image 20231210000142.png]]


#### Previewing Modes
1.  **Automatic**: Selects the optimal preview method based on the file type. Uses *IE*(Internet Explorer) for web-related files, displays text files in ASCII/Unicode, and opens recognized file types
2. **Text Mode**: Allows file contents to be previewed as ASCII or Unicode text. This mode is useful for revealing hidden text and binary data in non-text files


Use `Ctrl + F` to search for specific text within a file while in either text or hex preview mode.

![FTK Imager Previewing Mode - Find Xiaomi in Hex mode](https://tryhackme-images.s3.amazonaws.com/user-uploads/63da722f2d207d0049da10b1/room-content/8ad39dd0de241fae81acedf698579775.png)

  

FTK Imager: Recovering Deleted Files and Folders

To view and recover deleted files, expand directories in the File List pane and Evidence Tree pane. Right-click and select `Export Files` on individual files marked with an "x" icon or on entire directories/devices for bulk recovery of files (whether deleted or not).

# C# Shells and C2's
###### Objectives
- Foundations of analysing malware samples safely
- Fundamentals of .NET binaries
- The dnSpy tool for decompiling malware samples written in `.NET`
	- Building an essential methodology for analysing malware source code.

### Handling 101
**Sandbox**: Act's as a pretend computer setup like a real one.
	These are essential when conducting analysis becuase it stops experts from running malware on their actual work computers.

- **Network COntrols**: 
	- Limit and monitor traffic that the malware generates
	- Prevents propagation etc.
- **Visualization**:
	- run the malware in a controller environment like `remnux`
	- Use VMWare, VirtualBox, Hyper-v etc.
	- Isolated
	- Easy snapshots
- **Monitoring and Logging**: Sandboxes record detaled logs of malware's activities, including system interactions, network traffic, and file modification. These logs are invaluable for analysing and understanding the malware's behaviour

Unlike C/C++, languages that use .NET, such as `C#` don't directly translate the code into machine code after compilation. Instead, they use an intermediate Language (IL),  I.e, Pseudocode. Which is then translated into native maachine code during runtime via a Common Language Runtime (`CLR`) environment.


### **Namespaces, classes, functions and variables**
```C#
namespace DemoOnly
{
	internal class BasicProgamming
	{
		static voic Main(string[] args)
		{
		string to_print = "Very classy, never trashy";
		ShowOutput(to_print);
		}
	


		public static void ShowOutput(string text)
		{
			//print contents
			Console.WritLine(text);
	
		}
	}
}
```
### C# Syntax table and explainations 
#csharp 

| Code Syntax | Details |
|:-:|:-:|
|**Namespace**|A container that organises related code elements, such as classes, into a logical grouping. It helps prevent naming conflicts and provides structure to the code. In this example, the namespace `DemoOnly` is the namespace that contains the `BasicProgramming` class.|
|**Class**|Defines the structure and behaviour (through functions or methods) of the objects it contains. In this example, `BasicProgramming` is a class that includes the `Main` function and the `ShowOutput` function. Moreover, the `Main` function is the program's entry point, where the program starts its execution.|
|**Function**|A reusable block of code that performs a specific task or action. In this example, the `ShowOutput` function takes a string (through the `text` argument) as an input and uses it on `Console.WriteLine` to print it as its output. Note that the `ShowOutput` function only receives one argument based on how it is written.|
|**Variable**|A named storage location that can hold data, such as numbers (integers), text (strings), or objects. In this example, `to_print` is a variable that handles the text: "Hello World!"|






































































