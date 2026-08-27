# Threat Event (Unauthorized TOR Usage)
**Unauthorized TOR Browser Installation and Use**

## Steps the "Bad Actor" took Create Logs and IoCs:
1. Download the TOR browser installer: https://www.torproject.org/download/
2. Install it silently: ```tor-browser-windows-x86_64-portable-14.5.8.exe /S```
3. Opens the TOR browser from the folder on the desktop
4. Connect to TOR and browse a few sites. For example:
   - **The links to onion sites change a lot and these may have changed.**
   - Current Dread Forum: ```dreadytofatroptsdj6io7l3xptbet6onoyno2yv7jicoxknyazubrad.onion```
   - Dark Markets Forum: ```dreadytofatroptsdj6io7l3xptbet6onoyno2yv7jicoxknyazubrad.onion/d/DarkNetMarkets```
   - Current Elysium Market: ```elysiumutkwscnmdohj23gkcyp3ebrf4iio3sngc5tvcgyfp4nqqmwad.top/login```

6. Created a folder on my desktop called ```tor-shopping-list.txt``` and put a few fake (illicit) items in there
7. Delete the file.

---

## Tables Used to Detect IoCs:
| **Parameter**       | **Description**                                                              |
|---------------------|------------------------------------------------------------------------------|
| **Name**| DeviceFileEvents|
| **Info**|https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-deviceinfo-table|
| **Purpose**| Used for detecting TOR download and installation, as well as the shopping list creation and deletion. |

| **Parameter**       | **Description**                                                              |
|---------------------|------------------------------------------------------------------------------|
| **Name**| DeviceProcessEvents|
| **Info**|https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-deviceinfo-table|
| **Purpose**| Used to detect the silent installation of TOR as well as the TOR browser and service launching.|

| **Parameter**       | **Description**                                                              |
|---------------------|------------------------------------------------------------------------------|
| **Name**| DeviceNetworkEvents|
| **Info**|https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-devicenetworkevents-table|
| **Purpose**| Used to detect TOR network activity, specifically tor.exe and firefox.exe making connections over ports to be used by TOR (9001, 9030, 9040, 9050, 9051, 9150).|

---

## Related Queries:
```kql
// Installer name == tor-browser-windows-x86_64-portable-14.5.8.exe
// Detect the installer being downloaded
DeviceFileEvents
| where DeviceName == "vm-onboard-acha"
| where InitiatingProcessAccountName == "achost"
| where FileName contains "Tor"
| where Timestamp >= datetime(2025-10-15T21:38:45.248943Z)
| order by Timestamp desc
| project Timestamp, DeviceName, ActionType, FileName, FolderPath, SHA256, Account = InitiatingProcessAccountName


// TOR Browser being silently installed
DeviceProcessEvents
| where DeviceName == "vm-onboard-acha"
| where ProcessCommandLine contains "tor-browser-windows-x86_64-portable-14.5.8.exe  /S"
| project Timestamp, DeviceName, AccountName, ActionType, FileName, FolderPath, SHA256,  ProcessCommandLine


// TOR Browser or service was successfully installed and is present on the disk
DeviceProcessEvents
| where DeviceName == "vm-onboard-acha"
| where FileName has_any ("tor.exe", "Browser", "tor-browser", “firefox.exe”)
| project Timestamp, DeviceName, AccountName, ActionType, FileName, FolderPath, SHA256, ProcessCommandLine
| order by Timestamp desc


// TOR Browser or service was launched
DeviceProcessEvents
| where DeviceName == "vm-onboard-acha"
| where FileName has_any ("tor.exe", "Browser", "tor-browser", “firefox.exe”)
| project Timestamp, DeviceName, AccountName, ActionType, FileName, FolderPath, SHA256, ProcessCommandLine

// TOR Browser or service is being used and is actively creating network connections
DeviceNetworkEvents
| where DeviceName == "vm-onboard-acha"
| where InitiatingProcessAccountName != "system"
| where RemotePort in ("9001", "9030", "9040", "9050", "9051", "9150")
| project Timestamp, DeviceName, InitiatingProcessAccountName, ActionType, RemoteIP, RemotePort, RemoteUrl, InitiatingProcessFileName, InitiatingProcessFolderPath
| order by Timestamp desc


// User shopping list was created and, changed, or deleted
DeviceFileEvents
| where FileName contains "shopping-list.txt"
```

---

## Created By:
- **Author Name**: Ti Acha
- **Author Contact**: https://www.linkedin.com/in/tibon-a-8372a3227/
- **Date**: October 18, 2025
