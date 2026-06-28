# wazuh-threat-hunting-certutil-process-creation

## Overview

This lab demonstrates how to use Wazuh Threat Hunting together with Microsoft Sysmon to detect suspicious Windows process execution.

The objective was to simulate execution of **certutil.exe**, verify that Sysmon generated Event ID 1 (Process Creation), and investigate the event using Wazuh.

This lab mimics a common SOC investigation involving LOLBins (Living-off-the-Land Binaries).

---

## Lab Environment

Host Machine

- Windows 11
- Wazuh Agent
- Microsoft Sysmon
- PowerShell

Server

- Wazuh Manager
- Wazuh Dashboard

---

## MITRE ATT&CK

Technique:

T1218 - System Binary Proxy Execution

Utility:

certutil.exe

---

## Objective

Generate a Sysmon Process Creation event.

Verify:

- Sysmon logs the event
- Wazuh Agent is running
- Wazuh receives the event
- Event can be investigated from Threat Hunting

---

## Steps Performed

### 1. Verified Wazuh Agent

```powershell
Get-Service WazuhSvc
```

Status:

Running

---

### 2. Verified Sysmon

```powershell
Get-Service Sysmon64
```

Status:

Running

---

### 3. Generated Activity

Opened Command Prompt

Executed:

```cmd
certutil.exe
```

This generated a Sysmon Process Creation event.

---

### 4. Verified Sysmon Event

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=1)]]" `
-MaxEvents 5
```

Observed:

- Event ID 1
- Image:

```
C:\Windows\SysWOW64\certutil.exe
```

---

### 5. Investigated Wazuh

Opened

Threat Hunting

Reviewed incoming events.

Validated that:

- Wazuh Agent is online
- Sysmon operational log is working
- Events are searchable

---


## Detection Logic

Process Creation

↓

Sysmon Event ID 1

↓

Windows Event Log

↓

Wazuh Agent

↓

Wazuh Manager

↓

Threat Hunting

↓

SOC Investigation

---

## Skills Demonstrated

- Windows Event Analysis
- Sysmon
- Process Creation Investigation
- Threat Hunting
- Wazuh
- SOC Investigation
- MITRE ATT&CK Mapping
- Living-off-the-Land Binary Analysis

---

## Outcome

Successfully generated a Sysmon Process Creation event using certutil.exe, validated Windows logging, confirmed Wazuh agent operation, and investigated the event within Wazuh Threat Hunting.
