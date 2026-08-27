
<img width="400" src="https://github.com/user-attachments/assets/44bac428-01bb-4fe9-9d85-96cba7698bee" alt="Tor Logo with the onion and a crosshair on it"/>

# Threat Hunt Report: Unauthorized TOR Usage
- [Scenario Creation](https://github.com/Tacha8/threat-hunting-scenario-tor/blob/Tacha8/joshcybertest/threat-hunting-scenario-tor-event-creation.md)

## Platforms and Languages Leveraged
- Windows 10 Virtual Machines (Microsoft Azure)
- EDR Platform: Microsoft Defender for Endpoint
- Kusto Query Language (KQL)
- Tor Browser

##  Scenario.

Management suspects that some employees may be using TOR browsers to bypass network security controls because recent network logs show unusual encrypted traffic patterns and connections to known TOR entry nodes. Additionally, there have been anonymous reports of employees discussing ways to access restricted sites during work hours. The goal is to detect any TOR usage and analyze related security incidents to mitigate potential risks. If any use of TOR is found, notify management.

### High-Level TOR-Related IoC Discovery Plan

- **Check `DeviceFileEvents`** for any `tor(.exe)` or `firefox(.exe)` file events.
- **Check `DeviceProcessEvents`** for any signs of installation or usage.
- **Check `DeviceNetworkEvents`** for any signs of outgoing connections over known TOR ports.

---

## Steps Taken

### 1. Searched the `DeviceFileEvents` Table

Searched the DeviceFileEvents table for ANY file that had the string “tor” in it and discovered what looks like the user “achost” downloaded a tor installer, did something that resulted in many tor-related files being copied to the desktop and the creation of a file called “tor-shopping-list.txt” on the desktop at 2025-10-15T22:14:30.3912294Z. These events began at: 2025-10-15T21:38:45.248943Z

**Query used to locate events:**

```kql
DeviceFileEvents
| where DeviceName == "vm-onboard-acha"
| where InitiatingProcessAccountName == "achost"
| where FileName contains "Tor"
| where Timestamp >= datetime(2025-10-15T21:38:45.248943Z)
| order by Timestamp desc
| project Timestamp, DeviceName, ActionType, FileName, FolderPath, SHA256, Account = InitiatingProcessAccountName

```
<img width="1195" height="698" alt="image" src="https://github.com/user-attachments/assets/8c5e03bf-f0e2-48fb-9872-2a1260a2d8dc" />


---

### 2. Searched the `DeviceProcessEvents` Table

Searched the DeviceProcessEvents table for any ProcessCommandLine that contained the string “tor-browser-windows-x86_64-portable-14.5.8.exe”. On logs returned at 2025-10-15 T17:01:33 PM on the system vm-onboard-acha, user achost spawned a new process. The executable launched was tor-browser-windows-x86_64-portable-14.5.8.exe. The command line used to launch the process was simply the executable name itself.

**Query used to locate event:**

```kql

DeviceProcessEvents
| where DeviceName == "vm-onboard-acha"
| where ProcessCommandLine contains "tor-browser-windows-x86_64-portable-14.5.8.exe"
| project Timestamp, DeviceName, AccountName, ActionType, FileName, FolderPath, SHA256,  ProcessCommandLine

```
<img width="1211" height="553" alt="image" src="https://github.com/user-attachments/assets/cb261832-b19e-49e6-a146-5610eced2e07" />


---

### 3. Searched the `DeviceProcessEvents` Table for TOR Browser Execution

Searched the DeviceProcessEvents table for any indication that user “employee” actually opened the tor browser. There was evidence that they did open it at 2025-10-15T22:02:12.5405474Z There were several instances of firefox.exe with tor.exe

**Query used to locate events:**

```kql
DeviceProcessEvents
| where DeviceName == "vm-onboard-acha"
| where FileName has_any ("tor.exe", "Browser", "tor-browser", “firefox.exe”)
| project Timestamp, DeviceName, AccountName, ActionType, FileName, FolderPath, SHA256, ProcessCommandLine
| order by Timestamp desc

```
<img width="1227" height="569" alt="image" src="https://github.com/user-attachments/assets/30ff9327-5090-49dc-984c-65a5e572feed" />


---

### 4. Searched the `DeviceNetworkEvents` Table for TOR Network Connections

Searched the DeviceNetworkEvents table for any indication the tor browser was used to establish a connection using any of the known tor ports 
On 2025-10-15T22:02:21.738758Z, on device vm-onboard-acha, user achost successfully made a network connection. The initiating process was tor.exe, connecting to the remote IP address 151.242.132.118 on port 9001. There were a few other connections to sites over port 443.

**Query used to locate events:**

```kql

DeviceNetworkEvents
| where DeviceName == "vm-onboard-acha"
| where InitiatingProcessAccountName != "system"
| where RemotePort in ("9001", "9030", "9040", "9050", "9051", "9150")
| project Timestamp, DeviceName, InitiatingProcessAccountName, ActionType, RemoteIP, RemotePort, RemoteUrl, InitiatingProcessFileName, InitiatingProcessFolderPath
| order by Timestamp desc

```
<img width="1226" height="610" alt="image" src="https://github.com/user-attachments/assets/1a16c7fa-8092-425a-a7a3-ac7b12c92057" />


---

## Chronological Event Timeline 

 Step 1 – Installer execution

 On 2025-10-15 at ~17:01:33 (local time), on device vm-onboard-acha, user achost launched the executable tor-browser-windows-x86_64-portable-14.5.8.exe. The full command line was simply the executable name.

 (This indicates the user initiated the portable version of Tor Browser.)
 
 Step 2 – File activities begin

 At 2025-10-15T21:38:45.248943Z, file events related to “tor” (files whose names include “Tor”) began appearing in the DeviceFileEvents table for user achost on vm-onboard-acha.

 (This marks the starting point of file system activity tied to Tor usage.)
 
 Step 3 – Desktop file creation & setup

 By 2025-10-15T22:14:30.3912294Z, a file named tor-shopping-list.txt was created on the desktop, and numerous tor-related files were copied to the desktop by user achost.

 (This suggests the user staged or began using the Tor environment actively.)
 
 Step 4 – Tor browser/client launch

 At 2025-10-15T22:02:12.5405474Z, there is evidence in DeviceProcessEvents of processes such as tor.exe (and possibly firefox.exe in a Tor context) being executed by user achost on the device.

 (This is the point where the Tor client/browser appears to have been opened.)
 
 Step 5 – Network connection via Tor

 At 2025-10-15T22:02:21.738758Z, a network connection event: user achost on device vm-onboard-acha, with the initiating process tor.exe, successfully connected to remote IP 151.242.132.118 on port 9001.

 (Port 9001 is a known port commonly used by Tor clients/relays.)

---

## Implications

The usage of port 9001 is a well-known indicator of Tor network activity. 
The fact that a portable version of Tor Browser was used may indicate an attempt to avoid installation artifacts or use a non-managed browser environment.
The creation of a file named tor-shopping-list.txt and desktop file copies suggests manual user involvement rather than automated software distribution.
Because Tor provides anonymity and can be used to circumvent monitoring controls, this usage may represent a policy violation or security concern depending on environment rules.


---

## Summary

User achost on device vm-onboard-acha installed and launched the portable version of the Tor Browser. After the installer launch, several “tor”-named files appeared on the desktop, including a file named tor-shopping-list.txt. Shortly thereafter, the process tor.exe was executed and connected to remote IP 151.242.132.118 on port 9001, a port commonly associated with the Tor network.


---

## Response Taken

TOR usage was confirmed on the endpoint vm-onboard-acha by the user achost . The device was isolated and the user's direct manager was notified.

---
