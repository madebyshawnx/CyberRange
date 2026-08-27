# Architecture


## Overview

This document provides the high-level architecture for the Contoso Enterprise Identity Platform. The environment was designed to simulate a modern hybrid enterprise by integrating on-premises Active Directory with Microsoft Entra ID, Azure cloud services, identity automation, access management, and security monitoring.

Rather than focusing on a single technology, this project demonstrates how multiple identity and security components work together to provide centralized identity management, secure authentication, automated provisioning, least-privilege access, and continuous monitoring.

---

# Architecture Diagram

> **Enterprise Identity Platform**

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/81fa6ef8-1037-451d-9606-eaa7e8bd4057" />


---

# Project Relationships

Each folder in this repository represents a component of the overall enterprise identity platform.

| Project | Purpose |
|----------|---------|
| Enterprise Active Directory | On-premises identity infrastructure |
| Microsoft Entra ID | Cloud identity management |
| Hybrid Identity | Identity synchronization |
| Azure RBAC | Authorization and least privilege |
| Conditional Access | Identity protection and authentication policies |
| Privileged Identity Management | Administrative identity security |
| Identity Governance | Identity lifecycle and compliance |
| Enterprise Applications | Application identity integration |
| PowerShell Automation | Automated identity provisioning |
| Identity Attack Lab | Identity attack simulation |
| Microsoft Defender XDR | Endpoint detection and response |
| Microsoft Sentinel | SIEM and threat hunting |
| Zero Trust | Identity security architecture |

---

# Architecture Components

## Microsoft Azure

Azure provides the cloud infrastructure used to host identity services, role-based access control, enterprise resources, and security services.

---

## Microsoft Entra ID

Microsoft Entra ID serves as the cloud identity provider for authentication, authorization, and identity management.

Responsibilities include:

- Cloud identity management
- Authentication
- Multi-Factor Authentication (MFA)
- Conditional Access
- Azure RBAC
- Identity synchronization

---

## Azure RBAC

Azure Role-Based Access Control (RBAC) implements least-privilege access by assigning permissions through built-in Azure roles.

Examples include:

- Virtual Machine Administrator
- Reader
- Contributor
- Resource Group permissions

---

## Conditional Access

Conditional Access evaluates authentication requests before access is granted.

Policies implemented include:

- Require MFA for administrators
- Require MFA for all users
- Block legacy authentication
- Block high-risk sign-ins
- Block sign-ins from high-risk countries
- Require compliant devices for privileged administrators

---

## Hybrid Identity (Cloud Sync)

Microsoft Entra Cloud Sync securely synchronizes identities between the on-premises Active Directory environment and Microsoft Entra ID.

Hybrid identity provides:

- Unified identities
- Password synchronization
- Centralized authentication
- Cloud resource access

---

## Active Directory Domain Services

The on-premises Active Directory environment provides centralized identity administration.

Implemented components include:

- Organizational Units (OUs)
- Users
- Security Groups
- Group Policy Objects (GPOs)
- Administrative accounts

---

## PowerShell Joiner-Mover-Leaver Automation

PowerShell automates the identity lifecycle by provisioning and managing user accounts based on HR-driven processes.

Automated tasks include:

- User provisioning
- Department placement
- Security group assignment
- Account updates
- Account deprovisioning

---

## Enterprise Resources

Enterprise resources represent the systems protected by the identity platform.

Examples include:

- File servers
- Business applications
- Azure resources
- Databases
- Internal services

---

## Microsoft Defender XDR

Microsoft Defender XDR provides endpoint detection and response (EDR) capabilities.

Functions include:

- Threat detection
- Endpoint investigation
- Incident response
- Endpoint telemetry

---

## Microsoft Sentinel

Microsoft Sentinel serves as the Security Information and Event Management (SIEM) platform.

Capabilities include:

- Log collection
- Threat hunting
- KQL queries
- Analytics rules
- Incident investigation

---

# Authentication & Access Flow

1. User identities are created and managed within Active Directory.
2. Microsoft Entra Cloud Sync synchronizes identities to Microsoft Entra ID.
3. Users authenticate through Microsoft Entra ID.
4. Conditional Access evaluates authentication requests using identity, device, location, and risk signals.
5. Azure RBAC authorizes access to enterprise resources using least-privilege principles.
6. Microsoft Defender XDR and Microsoft Sentinel continuously monitor the environment for security events and threats.

---

# Security Principles

The architecture was designed around modern identity security principles:

- Hybrid Identity
- Zero Trust
- Least Privilege
- Multi-Factor Authentication (MFA)
- Identity Lifecycle Management
- Defense in Depth
- Continuous Monitoring
- Risk-Based Authentication

---

# Technologies Used

### Identity

- Active Directory Domain Services
- Microsoft Entra ID
- Microsoft Entra Cloud Sync

### Cloud

- Microsoft Azure
- Azure RBAC

### Security

- Microsoft Entra Conditional Access
- Microsoft Defender XDR
- Microsoft Sentinel

### Automation

- PowerShell
- Terraform

---

# Conclusion

The Contoso Enterprise Identity Platform demonstrates how modern organizations integrate identity, authentication, authorization, automation, and security into a unified hybrid identity architecture.

Each project within this repository builds upon the previous one to simulate an enterprise environment that aligns with Microsoft identity and Zero Trust best practices.
