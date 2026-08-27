# Active-Directory-Kerberos-Authentication-SQL-Service-Account-Security-Lab


This project demonstrates how Kerberos authentication works within an Active Directory environment by deploying Microsoft SQL Server under a dedicated domain service account, configuring Service Principal Names (SPNs), and validating Kerberos service ticket issuance. Throughout the project, multiple authentication and networking issues were identified and resolved, including SQL connectivity, TCP/IP configuration, Windows Firewall rules, SQL authentication, and SPN registration.

The project concludes with successful Kerberos authentication from a domain workstation and analysis of the resulting Kerberos service ticket used by SQL Server. 

## Network Topology

<img width="476" height="588" alt="image" src="https://github.com/user-attachments/assets/d7c7c9e1-49aa-45a7-951c-7957919e42ea" />


---

### Technologies Used
- Active Directory
- Kerberos
- DNS
- DHCP
- SQL Server 2022
- SQL Server Management Studio
- Windows Authentication
- Service Principal Names
- Windows Firewall
- PowerShell
- Command Prompt
- Event Viewer

--- 

### Active Directory Deployment

Configured Active Directory Domain Services and promoted the server to a Domain Controller

<img width="657" height="608" alt="image" src="https://github.com/user-attachments/assets/4c62495b-d8be-41fc-958d-1422ac7a3b84" />


Configured:

- Active Directory Domain Services
- DNS
- DHCP
- Organizational Units
- Domain Users
- Security Groups



---

## BP 2 - SQL Server Deployment


<img width="677" height="607" alt="image" src="https://github.com/user-attachments/assets/661de2f3-e94b-44d0-99c3-848729add031" />


Installed:

- SQL Server 2022 Developer

Configured SQL Server Database Engine to run under:

- ACHATECH\SQLService

instead of the default virtual service account.

---

## BP 3 - SQL Networking

<img width="590" height="413" alt="image" src="https://github.com/user-attachments/assets/50f8001b-3788-44ee-a3b5-3b5b128c57f0" />



Enabled:

TCP/IP
SQL Server Port 1433

Validated connectivity using:

netstat

---

## BP 4 - Windows Firewall Troubleshooting

<img width="812" height="572" alt="image" src="https://github.com/user-attachments/assets/c32f9582-333c-405d-b47c-5070fd7909a9" />

<img width="600" height="270" alt="image" src="https://github.com/user-attachments/assets/a187b1d4-da5c-42f5-ba8e-9539577d4bbb" />

Initial testing showed SQL Server was unreachable remotely.

Investigation determined Windows Defender Firewall was blocking inbound SQL traffic.

Created an inbound rule allowing TCP 1433.

Validated connectivity using:

- Test-NetConnection

---

## BP 5 - Service Principal Name Configuration

<img width="802" height="102" alt="image" src="https://github.com/user-attachments/assets/27594a77-7729-42b9-9983-3f327cd4b8f1" />

Initially, only the following SPN existed:

- MSSQLSvc/DC01

Clients connected using the FQDN:

- DC01.achatech.local

which prevented successful Kerberos service ticket issuance.

Using Microsoft documentation, additional SPNs were registered:

- MSSQLSvc/DC01.achatech.local

- MSSQLSvc/DC01.achatech.local:1433

Verification:

- setspn -Q MSSQLSvc/*

---

## BP 6 - Kerberos Authentication Validation

<img width="635" height="592" alt="image" src="https://github.com/user-attachments/assets/a738063e-8481-4070-8120-3bb5398255ba" />

Connected from:

- SalesPC01

using:

- Windows Authentication


<img width="958" height="520" alt="image" src="https://github.com/user-attachments/assets/65a7766a-9e0c-4269-96df-b4d43073d913" />

Verified Kerberos ticket generation using:

- klist

Observed:

- MSSQLSvc/DC01.achatech.local:1433

confirming successful Kerberos authentication.

---

## Authentication Flow 

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/f135bb46-d788-4777-a0b9-337d3fa56677" />

---


## Full Troubleshooting Summary 

During the project, the following issues were identified and resolved:

| Issue | Resolution |
| :---- | :--------- |
| SQL TCP/IP disabled | Enabled TCP/IP and restarted SQL Server |
| Remote SQL connections blocked | Created Windows Firewall rule for TCP 1433 |
| SQL login failure | Created Windows-authenticated SQL login |
| Kerberos ticket not generated | Registered missing FQDN SPNs |
| SQL connectivity verification | Validated connectivity using `Test-NetConnection` |

--- 

## Skills Demonstrated

- Active Directory Administration
- Windows Server Administration
- Kerberos Authentication
- SQL Server Administration
- Service Principal Names (SPNs)
- Windows Authentication
- Windows Firewall Administration
- Authentication Troubleshooting
- Network Troubleshooting
- PowerShell
- SQL Server Management Studio
- Microsoft Documentation Research

---

## Key Takeaways

This project provided hands-on experience with enterprise authentication by deploying a Kerberos-enabled SQL Server environment from scratch. Beyond the initial configuration, the project involved troubleshooting authentication, networking, and SPN-related issues that prevented Kerberos service ticket generation. Resolving those issues reinforced the importance of understanding how Active Directory, Kerberos, SQL Server, and networking components interact in a Windows enterprise environment. By analyzing the complete Kerberos authentication workflow, I developed a deeper understanding of how service accounts authenticate within Active Directory and why they are targeted by Kerberoasting attacks. The purpose was reinforce the importance of proper SPN configuration, strong service account credentials, managed service accounts (gMSAs), and monitoring Kerberos service ticket requests (Event ID 4769) to help detect and mitigate potential Kerberoasting activity.

<img width="952" height="878" alt="image" src="https://github.com/user-attachments/assets/a3d02a3c-931d-4bc9-bcac-e31e34ebcbaa" />

