# Enterprise Endpoint Compromise Investigation and Attack Reconstruction

## Overview

This project documents an enterprise endpoint compromise investigation conducted using Microsoft Defender XDR and Kusto Query Language (KQL). The objective was to identify malicious activity hidden within enterprise endpoint telemetry, reconstruct the attack lifecycle, determine the extent of the compromise, and evaluate the effectiveness of Microsoft Defender security controls.

During the investigation, I analyzed authentication events, process execution, PowerShell activity, registry modifications, file system changes, Windows Defender telemetry, and API call events to distinguish malicious activity from legitimate system behavior.

---

## Objectives

- Investigate a suspected enterprise endpoint compromise.
- Reconstruct the attack timeline from initial access through post-exploitation.
- Identify attacker persistence, defense evasion, and credential access techniques.
- Analyze Microsoft Defender XDR telemetry using Kusto Query Language (KQL).
- Evaluate whether Microsoft Defender detections resulted in successful prevention.
- Produce evidence-based findings to support incident response and remediation.

---

## Technologies Used

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Kusto Query Language (KQL)

---

## Data Sources

- DeviceEvents
- DeviceProcessEvents
- DeviceRegistryEvents
- DeviceFileEvents
- DeviceLogonEvents

---

## Investigation Scope

| Item | Value |
|------|-------|
| Investigation Window | 2025-12-13 09:48:40 UTC – 2025-12-13 10:48:40 UTC |
| Endpoint | azwks-phtg-01 |
| User Account | vmadminusername |
| Investigation Focus | Enterprise endpoint compromise assessment |

---

# Investigation Summary

The investigation identified a successful enterprise endpoint compromise that progressed from unauthorized authentication into multiple stages of post-exploitation.

Following successful authentication through password reuse, the attacker executed PowerShell payloads, established multiple persistence mechanisms, modified Microsoft Defender exclusions, communicated with external command-and-control infrastructure, and attempted credential access by interacting with the Local Security Authority Subsystem Service (LSASS).

By correlating telemetry across multiple Microsoft Defender XDR data sources, I reconstructed the complete attack timeline, validated attacker persistence, confirmed defense evasion activity, identified credential access techniques, and assessed Microsoft's security controls to determine whether detections resulted in successful prevention.

## Investigation Evidence

The screenshot below shows Microsoft Defender XDR Advanced Hunting used during the investigation to analyze endpoint telemetry using Kusto Query Language (KQL). The query identified PowerShell requesting elevated access to the Local Security Authority Subsystem Service (LSASS), followed by memory read operations consistent with credential access activity.

<img width="912" height="446" alt="image" src="https://github.com/user-attachments/assets/a2da34cd-d197-40da-9547-9fe188751ca6" />

---

# Key Findings

## Initial Access

- Confirmed successful authentication following repeated failed logon attempts.
- Determined the compromise resulted from password reuse rather than password spraying or brute-force attacks.

---

## Persistence

The attacker established multiple persistence mechanisms to maintain access if one method failed.

Persistence mechanisms included:

- Registry Run Key persistence
- Startup Folder shortcut
- PowerShell startup launcher
- Custom Windows Application Event Log registration

Startup persistence was validated by confirming multiple executions of the malicious startup PowerShell script during the investigation.

---

## Defense Evasion

The investigation identified several techniques designed to reduce security visibility and avoid detection.

These techniques included:

- Hidden PowerShell execution
- Execution Policy Bypass
- Temporary Microsoft Defender exclusions
- Persistent Microsoft Defender exclusions
- Process execution through `cmd.exe` to obscure process lineage
- Hidden directories and files

One particularly effective technique temporarily added a Microsoft Defender exclusion immediately before payload execution and removed it immediately afterward, minimizing Defender inspection without leaving a permanent configuration change.

---

## Credential Access

Analysis confirmed suspicious access to the Local Security Authority Subsystem Service (LSASS).

Evidence showed PowerShell requesting progressively higher process access rights before successfully reading LSASS memory. This behavior is consistent with credential access techniques commonly used to obtain cached authentication material.

---

## Microsoft Defender Assessment

Microsoft Defender successfully generated detection telemetry for multiple malicious artifacts throughout the investigation.

However, telemetry analysis also demonstrated that several persistence mechanisms remained active despite detection events. This reinforces the importance of validating defensive outcomes rather than assuming detection alone prevented attacker activity.

---

## Skills Demonstrated

- Threat Hunting
- Digital Forensics
- Incident Response
- Attack Timeline Reconstruction
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Kusto Query Language (KQL)
- Windows Endpoint Analysis
- Endpoint Detection and Response (EDR)
- Threat Detection Engineering
- MITRE ATT&CK Mapping

# Technical Investigation Walkthrough

## Phase 1 – Initial Access Investigation

The investigation began by validating authentication activity to determine how the attacker gained access to the endpoint.

Using Microsoft Defender XDR logon telemetry, I analyzed successful and failed authentication attempts during the investigation window. Correlating authentication events with user activity confirmed that the endpoint was successfully accessed after multiple failed logon attempts.

### Findings

- Successful authentication identified.
- Password reuse determined to be the initial access vector.
- No evidence of password spraying or brute-force attacks.

---

## Phase 2 – Post-Compromise Activity

Following successful authentication, I investigated process execution to identify attacker activity occurring immediately after logon.

Process telemetry revealed multiple PowerShell executions using hidden windows and execution policy bypass techniques designed to reduce visibility while executing malicious scripts.

### Findings

- Hidden PowerShell execution observed.
- ExecutionPolicy Bypass used.
- Operator PowerShell scripts identified.
- Workspace established under:

```
C:\ProgramData\PHTG\HealthCloud
```

---

## Phase 3 – Persistence Analysis

The next phase focused on determining whether the attacker established persistence.

Registry, startup folder, and process telemetry confirmed several independent persistence mechanisms designed to survive user logoff and system reboot.

### Findings

Persistence mechanisms included:

- Registry Run Key
- Startup Folder shortcut
- PowerShell startup launcher
- Custom Windows Event Log source

Repeated execution of the startup PowerShell script confirmed persistence was functioning successfully.

---

## Phase 4 – Defense Evasion Investigation

I analyzed Microsoft Defender configuration changes and PowerShell behavior to determine whether the attacker attempted to bypass endpoint protection.

Investigation confirmed multiple defense evasion techniques.

### Findings

- Hidden PowerShell execution
- ExecutionPolicy Bypass
- Temporary Defender exclusions
- Persistent Defender exclusions
- Hidden directories
- Process lineage obfuscation using cmd.exe

One notable finding showed the attacker temporarily adding a Microsoft Defender exclusion immediately before payload execution before removing the exclusion shortly afterward, minimizing security inspection while avoiding a permanent Defender configuration change.

---

## Phase 5 – Command-and-Control Investigation

PowerShell scripts and decoded network artifacts were analyzed to identify attacker infrastructure.

### Findings

Command-and-control communications were established with external infrastructure including:

- status.health-cloud.cc
- updates.health-cloud.cc

PowerShell scripts communicated with API endpoints responsible for beaconing and operator tasking.

This redundant communication strategy increased resiliency if one command-and-control server became unavailable.

---

## Phase 6 – Credential Access Investigation

The final stage of the investigation focused on identifying evidence of credential theft.

API call telemetry showed PowerShell requesting progressively higher access rights to the Local Security Authority Subsystem Service (LSASS).

Further investigation confirmed successful memory reads against LSASS.

### Findings

- PowerShell opened LSASS.
- Full process access requested.
- ReadProcessMemory API calls confirmed.
- Activity consistent with credential access techniques.

---

## Phase 7 – Microsoft Defender Effectiveness Assessment

The final step evaluated whether Microsoft Defender successfully prevented attacker activity.

Microsoft Defender generated detection telemetry for multiple malicious artifacts throughout the investigation.

However, telemetry also confirmed that several persistence mechanisms remained active despite antivirus detections.

### Assessment

Microsoft Defender successfully detected malicious activity but did not consistently prevent persistence from remaining active.

This investigation demonstrates the importance of validating security control effectiveness through telemetry analysis rather than assuming detections result in successful prevention.

---

# MITRE ATT&CK Techniques Observed

| Tactic | Technique |
|---------|-----------|
| Initial Access | Valid Accounts (T1078) |
| Execution | PowerShell (T1059.001) |
| Persistence | Registry Run Keys / Startup Folder (T1547.001) |
| Defense Evasion | Indicator Removal & Defender Exclusions (T1562) |
| Command and Control | Application Layer Protocol (T1071) |
| Credential Access | OS Credential Dumping (T1003.001) |

---

# Conclusion

This investigation reconstructed the complete attack lifecycle from initial access through post-exploitation using Microsoft Defender XDR telemetry and Kusto Query Language.

By correlating authentication events, process execution, registry modifications, Defender telemetry, API call events, and persistence artifacts, I successfully identified attacker techniques, validated the effectiveness of endpoint security controls, and reconstructed the sequence of attacker actions.

This project demonstrates practical experience in enterprise threat hunting, digital forensics, incident response, endpoint detection and response (EDR), and attack reconstruction using Microsoft Defender XDR.
