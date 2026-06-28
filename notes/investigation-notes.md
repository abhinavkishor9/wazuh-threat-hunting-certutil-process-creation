# Investigation Notes

## Investigation Summary

A simulated execution of **certutil.exe** was performed to validate endpoint visibility using Microsoft Sysmon and Wazuh.

The objective was to confirm that Sysmon generated a Process Creation event and that Wazuh successfully collected endpoint telemetry for investigation.

---

# Initial Observation

Execution:

```cmd
certutil.exe
```

Expected Detection:

Sysmon Event ID 1

---

# Verification

## Wazuh Agent

Verified:

```powershell
Get-Service WazuhSvc
```

Status:

Running

---

## Sysmon

Verified:

```powershell
Get-Service Sysmon64
```

Status:

Running

---

## Event Verification

Executed:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=1)]]"
```

Observed:

Event ID:

```
1
```

Image:

```
C:\Windows\SysWOW64\certutil.exe
```

Result:

Successful Process Creation event.

---

# Wazuh Investigation

Reviewed:

Threat Hunting

Observed:

- Endpoint active
- Sysmon operational logs available
- Security events searchable

---

# MITRE ATT&CK

Technique

T1218

System Binary Proxy Execution

---

# Risk Assessment

Severity:

Medium

Reason:

certutil.exe is a legitimate Windows utility frequently abused by attackers for:

- File download
- Payload decoding
- Certificate manipulation
- Living-off-the-Land activity

Although the execution was benign for lab purposes, identical activity in production should be investigated.

---

# Indicators Observed

Executable

```
certutil.exe
```

Event Source

Sysmon

Event ID

1

Host

Windows Endpoint

Detection

Process Creation

---

# Conclusion

The simulated activity successfully generated a Sysmon Event ID 1.

The endpoint telemetry confirmed:

- Sysmon functioning correctly
- Wazuh Agent operational
- Windows event collection successful

This validates the logging pipeline required for future threat hunting exercises.
