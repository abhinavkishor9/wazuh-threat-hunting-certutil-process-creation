# Troubleshooting Notes

## Issue 1

### certutil.exe generated no visible output

Reason

Running:

```cmd
certutil.exe
```

without arguments only launches the executable.

This is sufficient to generate a Sysmon Process Creation event.

Resolution

Confirmed Process Creation using:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=1)]]"
```

---

## Issue 2

### No Event ID 1 initially visible

Cause

Recent Sysmon events contained Image Load (Event ID 7) and Registry events (Event ID 13).

Resolution

Filtered specifically for:

```powershell
EventID = 1
```

Successfully located Process Creation events.

---

## Issue 3

### Wazuh Threat Hunting returned no results

Possible Causes

- Incorrect time range
- Incorrect search field
- Event indexing delay
- Missing filters

Resolution

Verified:

- Sysmon operational log
- Wazuh Agent service
- Agent online status
- Event generation time

---

## Issue 4

### Agent communication verification

Manager

```bash
sudo /var/ossec/bin/agent_control -l
```

Confirmed:

```
Agent 001 Active
```

---

## Issue 5

### Wazuh Agent status

Windows

```powershell
Get-Service WazuhSvc
```

Status

Running

---

## Issue 6

### Sysmon status

Windows

```powershell
Get-Service Sysmon64
```

Status

Running

---

## Lessons Learned

- Sysmon Event ID 1 confirms process creation.
- certutil.exe is a Living-off-the-Land Binary (LOLBin) frequently abused by attackers.
- Wazuh visibility depends on proper agent communication and event collection.
- Time range selection is critical during Threat Hunting.
- Direct verification with Windows Event Logs is an effective first troubleshooting step before investigating in Wazuh.
