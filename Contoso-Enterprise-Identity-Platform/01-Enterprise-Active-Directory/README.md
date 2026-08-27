# Enterprise Active Directory

## Overview

This project demonstrates the deployment and administration of an enterprise Active Directory Domain Services (AD DS) environment. Active Directory serves as the centralized identity provider for managing users, computers, security groups, organizational units (OUs), and Group Policy Objects (GPOs) within the Contoso Enterprise Identity Platform.

The environment was designed to simulate a production enterprise domain where identities are centrally managed before synchronizing to Microsoft Entra ID through a hybrid identity architecture.

---

# Objectives

The primary objectives of this project were to:

- Deploy Active Directory Domain Services
- Configure a domain controller
- Create Organizational Units (OUs)
- Manage enterprise users and groups
- Configure Group Policy Objects
- Configure DNS
- Prepare the environment for Hybrid Identity

---

# Enterprise Active Directory Architecture

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/fc0f4cb2-6822-4ec0-ad14-48b4ce4c0a75" />


---

# Active Directory Environment

### Infrastructure

- Windows Server 2022
- Active Directory Domain Services
- DNS Server
- Windows 11 Client

### Domain

```text
ACHATECH.LOCAL (contoso)
```

---

# Organizational Unit Structure

The domain was organized using Organizational Units (OUs) to logically separate users, computers, servers, and administrative resources.

Example organizational units include:

- HR
- Finance
- Sales
- IT
- Servers
- Workstations
- Identity Management

<img width="960" height="597" alt="image" src="https://github.com/user-attachments/assets/332d43af-8833-4dfe-8059-7532735f5621" />


---

# User & Security Group Administration

Enterprise user accounts and security groups were created to simulate departmental identity management.

Administrative tasks included:

- Creating users
- Resetting passwords
- Managing memberships
- Assigning department security groups
- Delegating permissions

### Screenshot

<img width="730" height="587" alt="image" src="https://github.com/user-attachments/assets/5c04ee8b-fa19-4fad-9c6b-15f6a0d2f3a9" />


---

# Group Policy Management

Group Policy Objects (GPOs) were configured to centrally manage security and administrative settings across the domain.

Examples include:

- Password policies (created and implemented post snapshot)
- Desktop configuration 
- Security settings 
- Administrative restrictions (created and implemented post snapshot)

<img width="854" height="630" alt="image" src="https://github.com/user-attachments/assets/0a92ee17-19c4-4b39-858c-24e870cc4dc8" />



---

# DNS Configuration

Active Directory relies on DNS for authentication and domain services.

Configured components include:

- Forward Lookup Zone
- Host (A) records
- Domain controller records
- Service (SRV) records

<img width="956" height="676" alt="image" src="https://github.com/user-attachments/assets/811f2828-d370-4a99-abad-d8cb663cc68e" />


---

# Domain-Joined Client

A Windows client was joined to the Active Directory domain to validate authentication and centralized management.

The domain-joined workstation receives authentication, Group Policy, and access permissions from the domain controller.

<img width="949" height="416" alt="image" src="https://github.com/user-attachments/assets/64ab7024-40e8-4791-8c86-7ec1c9e5393e" />

---

# Authentication Process

Authentication follows the standard Active Directory workflow:

1. User signs into a domain-joined workstation.
2. Active Directory validates credentials.
3. Kerberos authentication is performed.
4. Security group memberships are evaluated.
5. Group Policy Objects are applied.
6. Access is granted to authorized resources.

---

# Security Principles

The Active Directory environment demonstrates the following enterprise identity concepts:

- Centralized Identity Management
- Least Privilege
- Role-Based Access Control (RBAC)
- Organizational Separation
- Group-Based Administration
- Kerberos Authentication
- Group Policy Management

---

# Technologies Used

### Identity

- Active Directory Domain Services
- DNS
- Kerberos
- Organizational Units
- Security Groups

### Administration

- Active Directory Users and Computers
- Group Policy Management Console
- DNS Manager
- Server Manager

---

# Skills Demonstrated

- Active Directory Administration
- User Management
- Organizational Unit Design
- Security Group Administration
- Group Policy Management
- DNS Administration
- Kerberos Authentication
- Enterprise Identity Management

---

# Outcome

A fully functional enterprise Active Directory environment was deployed to provide centralized identity management for the Contoso Enterprise Identity Platform.

This implementation established the on-premises identity foundation used throughout subsequent projects, including Hybrid Identity, Microsoft Entra ID, Azure RBAC, Conditional Access, Identity Governance, and PowerShell-based identity automation.
