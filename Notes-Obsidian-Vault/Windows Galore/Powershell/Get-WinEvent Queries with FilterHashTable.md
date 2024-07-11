## Table of Contents

- [Cheatsheet: Creating Get-WinEvent Queries with FilterHashtable](#cheatsheet:\creating\get-winevent\queries\with\filterhashtable)
  - [Basic Structure](#Basic\Structure)
  - [Key Parameters](#Key\Parameters)
  - [Examples](#Examples)
    - [1. Basic Query for a Specific Log](#1.\Basic\Query\for\a\Specific\Log)
    - [2. Filter by Provider and Event ID](#2.\Filter\by\Provider\and\Event\ID)
    - [3. Filter by Time Range](#3.\Filter\by\Time\Range)
    - [4. Filter by Level and Keywords](#4.\Filter\by\Level\and\Keywords)
    - [5. Querying with User ID](#5.\Querying\with\User\ID)

Absolutely, I understand your request. Let's start with the first topic: Creating Get-WinEvent queries with FilterHashtable. Here's a cheatsheet for that.

---

# Cheatsheet: Creating Get-WinEvent Queries with FilterHashtable

## Basic Structure
```powershell
Get-WinEvent -FilterHashtable @{
    LogName = '<LogName>';
    ProviderName = '<ProviderName>';
    Id = <EventId>;
    StartTime = <DateTime>;
    EndTime = <DateTime>;
    Level = <Level>;
    Keywords = <Keyword>;
    UserID = <UserID>;
}
```

## Key Parameters
- **LogName**: Specifies the name of the log (e.g., 'Application', 'System').
- **ProviderName**: Name of the event provider (e.g., 'Microsoft-Windows-TaskScheduler').
- **Id**: Specifies the event ID.
- **StartTime**: Get events that were created after this time.
- **EndTime**: Get events that were created before this time.
- **Level**: Specifies the level of the event (e.g., Critical, Error, Warning, Information, Verbose).
- **Keywords**: Specifies keywords of the event.
- **UserID**: Get events with a specific user's SID.

## Examples

### 1. Basic Query for a Specific Log
```powershell
Get-WinEvent -FilterHashtable @{ LogName = 'System' }
```

### 2. Filter by Provider and Event ID
```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Application';
    ProviderName = 'Microsoft-Windows-TaskScheduler';
    Id = 101
}
```

### 3. Filter by Time Range
```powershell
$startTime = (Get-Date).AddDays(-1)
$endTime = Get-Date
Get-WinEvent -FilterHashtable @{
    LogName = 'Security';
    StartTime = $startTime;
    EndTime = $endTime
}
```

### 4. Filter by Level and Keywords
```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System';
    Level = 2; # Error
    Keywords = 0x80000000000000 # Audit Failure
}
```

### 5. Querying with User ID
```powershell
$userSID = (Get-LocalUser -Name "UserName").SID
Get-WinEvent -FilterHashtable @{
    LogName = 'Security';
    UserID = $userSID
}
```

---

This cheatsheet provides a basic guide to formulating `Get-WinEvent` queries using `FilterHashtable` in PowerShell. Remember, querying event logs is a powerful way to monitor system health and troubleshoot issues, so understanding how to effectively construct these queries is a valuable skill in system administration.