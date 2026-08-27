# Active-Directory-Enterprise-Administration

This project documents the deployment, administration, and troubleshooting of an enterprise-style Active Directory environment built in a virtualized lab. The objective was to gain hands-on experience with identity management, authentication, access control, Group Policy, DNS, and common enterprise troubleshooting scenarios.

The lab was designed to simulate a small corporate environment and demonstrate the technologies and workflows commonly used by Help Desk, System Administration, IAM, SOC, and Cybersecurity professionals.

<img width="518" height="456" alt="image" src="https://github.com/user-attachments/assets/4550cff1-1ff3-40e4-826a-7b385d02b75f" />

---

## Lab Architecture
### Environment
- Windows Server 2022 (Domain Controller)
- Windows 10 Pro (Domain Workstation)
- VirtualBox
- Active Directory Domain Services (AD DS)
- DNS
- Group Policy
- SMB File Shares
### Domain
- Domain Name: achatech.local
- Domain Controller: DC01
- Workstation: SALES-PC01
### Technologies Used
- Active Directory Domain Services
- DNS
- Group Policy
- Kerberos
- SMB
- NTFS
- Windows Server 2022
- Windows 10 Pro
- VirtualBox
- Event Viewer
### Skills Demonstrated
- Active Directory Administration
- Identity and Access Management (IAM)
- Role-Based Access Control (RBAC)
- DNS Administration
- Group Policy Management
- Windows Administration
- Authentication Troubleshooting
- Kerberos Analysis
- Event Log Analysis
- SMB File Sharing
- NTFS Permissions Management
- Root Cause Analysis
- Enterprise Troubleshooting

--- 

## Active Directory Deployment
Configured Active Directory Domain Services and promoted the server to a Domain Controller

## Organizational Units (OUs)

Created and organized the following OUs:

- HR
- Sales
- IT
- Finance
- Workstations
- Servers

This structure was used to logically separate users, computers, and administrative functions.

<img width="518" height="488" alt="image" src="https://github.com/user-attachments/assets/b2b51988-498b-4abc-b55a-177ab0ec16c9" />

<img width="334" height="349" alt="image" src="https://github.com/user-attachments/assets/1ae62447-35b7-4ddc-a554-31850e205462" />





---

## Identity and Access Management

### User Administration

Created and managed domain user accounts within Active Directory.

Examples:

- Mike Davis
- Chris Walker
- Security Groups

<img width="511" height="354" alt="image" src="https://github.com/user-attachments/assets/494d3f30-f4f9-4228-8d4d-df60838805f9" />


Created department-based security groups including:

- Sales_Users
- HR_Users
- IT_Admins
- Role-Based Access Control (RBAC)

<img width="515" height="451" alt="image" src="https://github.com/user-attachments/assets/6b2eee63-95b1-4466-bb8d-02f35e0ab5c7" />


Implemented RBAC by assigning permissions to security groups rather than individual users.

Benefits:

- Improved scalability
- Simplified administration
- Reduced permission misconfigurations
- Streamlined onboarding and offboarding processes

---

## DNS Configuration

Configured Active Directory-integrated DNS.

Verified:

- Forward Lookup Zones
- Internal domain name resolution
- Workstation communication with domain services

<img width="517" height="361" alt="image" src="https://github.com/user-attachments/assets/312b71f1-ff59-4559-ad13-8ef70e3e8acb" />


Demonstrated the critical dependency between Active Directory and DNS.

## Enterprise Workstation Deployment

Configured a Windows 10 Pro workstation and joined it to the domain.

<img width="517" height="461" alt="image" src="https://github.com/user-attachments/assets/47b1a6e8-0bc6-438f-a934-a2e9e08e5f54" />


Validated:

- Domain authentication
- Domain membership
- Communication with the Domain Controller
- Centralized identity management


## File Server and Access Control

### SMB Shares

<img width="519" height="346" alt="image" src="https://github.com/user-attachments/assets/2f1bc48a-9f49-4b14-a403-9e8d365d5b73" />


Created department-based file shares for:

- HR
- Sales
- IT
- Finance

### NTFS Permissions

<img width="521" height="436" alt="image" src="https://github.com/user-attachments/assets/816c0def-b62e-4e55-9e50-ba6a5054fdca" />


Configured:

- Read
- Write
- Modify
- Full Control

permissions using Active Directory security groups.

### Effective Permissions

Validated how SMB Share permissions and NTFS permissions combine to determine a user's effective access.

---

## Group Policy Administration

<img width="518" height="394" alt="image" src="https://github.com/user-attachments/assets/5e279620-df3c-4ecd-bc51-c428c2637ab8" />


Implemented centralized policy management using Group Policy.

### Password Policy

<img width="520" height="196" alt="image" src="https://github.com/user-attachments/assets/74832658-44d4-4b9b-9584-eba04adc10df" />


Configured:

- Password complexity requirements
- Minimum password length
- Password history enforcement

### Account Lockout Policy

<img width="517" height="179" alt="image" src="https://github.com/user-attachments/assets/22a20a15-4ed3-436d-8d09-fddee9aa0ad0" />

Configured:

- Account lockout threshold
- Lockout duration
- Reset counter settings

### Drive Mapping

Created a Group Policy Object that automatically mapped:

S: to \DC01\Sales

for Sales department users.

<img width="522" height="441" alt="image" src="https://github.com/user-attachments/assets/dcf839fb-276f-4d09-8f84-3c3b071f02a3" />

<img width="518" height="487" alt="image" src="https://github.com/user-attachments/assets/4b95548e-89b7-433b-be7f-1b624e47d5fc" />



## Authentication and Security Monitoring

### Event Viewer Analysis

<img width="519" height="484" alt="image" src="https://github.com/user-attachments/assets/a5c9393c-b6ff-44a7-94d6-6d2d2ac2a5a7" />

Investigated Windows Security logs and authentication events.

### Kerberos Authentication

<img width="517" height="483" alt="image" src="https://github.com/user-attachments/assets/96ff9357-734e-4d94-a941-0529e7c076b4" />

Analyzed Event ID 4771:

- Kerberos pre-authentication failure
- Failed domain authentication attempt
- Authentication troubleshooting workflow

### Security Monitoring Concepts

Reviewed:

- Authentication telemetry
- Failed authentication events
- Kerberos authentication processes
- Event log investigation


## Troubleshooting Scenarios

### DNS Troubleshooting

#### Issue

- Group Policy failed to process after the workstation DNS server was changed from the internal Active Directory DNS server to a public DNS server.

#### Investigation

<img width="519" height="389" alt="image" src="https://github.com/user-attachments/assets/e4082103-54c2-4fcc-b731-9cb0cd7349b6" />


Observed:

- Failed domain name resolution
- Failed Group Policy updates
- Internal resources no longer resolving

#### Root Cause

- The workstation was configured to use Google's public DNS service instead of the internal Active Directory DNS server.

#### Resolution

- Restored the correct DNS configuration and flushed the local DNS cache.

#### Validation

Confirmed:

- Successful domain name resolution
- Successful Group Policy processing
- Restored communication with Active Directory services

## Group Policy Troubleshooting

#### Issue

Users reported the Sales mapped drive (S:) was missing.

#### Investigation

<img width="518" height="342" alt="image" src="https://github.com/user-attachments/assets/5b848f1b-acfd-4f6b-af33-485d564a1e11" />


- Network connectivity
- Authentication
- SMB share accessibility
- User permissions

#### Used:

- gpresult /r to investigate Group Policy processing.

#### Root Cause

- The drive mapping configuration had been removed from the Group Policy Object.

#### Resolution

- Recreated the mapped drive policy.

#### Validation

<img width="517" height="478" alt="image" src="https://github.com/user-attachments/assets/e3f5d0a3-e072-4bfc-8b0b-20035d5bb1b6" />


- Confirmed automatic restoration of the mapped drive upon policy refresh.

---

## Key Takeaways

This lab reinforced the importance of foundational enterprise technologies that support cybersecurity operations. Concepts such as DNS, Active Directory, Kerberos, Group Policy, and access control are critical to understanding how authentication, authorization, and resource access function within enterprise environments.

The project also provided practical experience troubleshooting real-world issues involving DNS resolution, Group Policy processing, authentication, and file share access.
