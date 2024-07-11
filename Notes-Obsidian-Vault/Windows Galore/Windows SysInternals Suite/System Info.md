## Table of Contents

- [Sysinternals Suite](#sysinternals\suite)
  - [System Information](#System\Information)
    - [1. System Information (Sysinternals) - `PsInfo`](#1.\System\Information\(Sysinternals)\-\`PsInfo`)
      - [Usage](#Usage)
      - [Examples](#Examples)
    - [2. Disk and Volume Information - `DiskView`](#2.\Disk\and\Volume\Information\-\`DiskView`)
      - [Usage](#Usage)
      - [Examples](#Examples)
  - [Process Information](#Process\Information)
    - [3. Process Explorer](#3.\Process\Explorer)
      - [Usage](#Usage)
      - [Examples](#Examples)
    - [4. Process Monitor](#4.\Process\Monitor)
      - [Usage](#Usage)
      - [Examples](#Examples)
    - [5. ListDLLs](#5.\ListDLLs)
      - [Usage](#Usage)
      - [Examples](#Examples)
  - [Network Configuration Information](#Network\Configuration\Information)
    - [6. TCPView](#6.\TCPView)
      - [Usage](#Usage)
      - [Examples](#Examples)
    - [7. PsPing](#7.\PsPing)
      - [Usage](#Usage)
      - [Examples](#Examples)
    - [8. Whois](#8.\Whois)
      - [Usage](#Usage)
      - [Examples](#Examples)

# Sysinternals Suite 
This cheatsheet focuses on how to use the Sysinternals Suite for gathering system, process, and network configuration information.
## System Information

### 1. System Information (Sysinternals) - `PsInfo`

Provides detailed information about a system.

#### Usage
```
psinfo [-d] [-h] [-s] [-c] [-t] [[\\]computer[,computer[,...]]]
```

#### Examples
- **Local System Information**: `psinfo`
- **Remote System Information**: `psinfo \\RemoteComputer`
- **Detailed Information**: `psinfo -d` for detailed information including installed hotfixes.
- **CSV Output**: `psinfo -c` for CSV format output.

### 2. Disk and Volume Information - `DiskView`

Visualizes the disk sectors and clusters used by files and NTFS metadata.

#### Usage
```
diskview
```

#### Examples
- **Launch DiskView**: Run `diskview` and select a disk to visualize its usage.

## Process Information

### 3. Process Explorer

Provides detailed information about processes, including their resource usage and DLL dependencies.

#### Usage
```
procexp.exe
```

#### Examples
- **Investigate Process Details**: Launch `procexp.exe` and double-click on a process to see its properties.
- **Check CPU and Memory Usage**: View real-time CPU and memory usage for processes.

### 4. Process Monitor

Monitors real-time file system, registry, and process/thread activity.

#### Usage
```
procmon.exe
```

#### Examples
- **Capture Process Activity**: Run `procmon.exe` to start capturing process activity.
- **Use Filters**: Apply filters to narrow down the activity log to specific processes or actions.

### 5. ListDLLs

Shows all the DLLs loaded into running processes.

#### Usage
```
listdlls [-d dllname] [processname|pid]
```

#### Examples
- **DLLs for a Process**: `listdlls processname`
- **Search for a Specific DLL**: `listdlls -d kernel32.dll`

## Network Configuration Information

### 6. TCPView

Displays detailed listings of all TCP and UDP endpoints, including local and remote addresses and the state of TCP connections.

#### Usage
```
tcpview.exe
```

#### Examples
- **Monitor Network Connections**: Run `tcpview.exe` to see all active connections and listening ports.

### 7. PsPing

Performs network latency and bandwidth measurements.

#### Usage
```
psping [-t] [-i interval] [-n iterations] [[-l size] target]
```

#### Examples
- **Ping a Host**: `psping microsoft.com`
- **Measure Latency**: `psping -t 192.168.1.1`

### 8. Whois

Looks up information about domain or IP address ownership.

#### Usage
```
whois domainname
```

#### Examples
- **Domain Information**: `whois example.com`

---

