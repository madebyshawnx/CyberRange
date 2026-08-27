# Microsoft Entra ID

## Overview

This project demonstrates the deployment and administration of Microsoft Entra ID as the cloud identity platform within the Contoso Enterprise Identity Platform.

Microsoft Entra ID provides centralized cloud identity management, authentication, authorization, and access to Microsoft cloud services. This implementation establishes the cloud identity foundation that supports Hybrid Identity, Azure RBAC, Conditional Access, Identity Governance, and enterprise application integration.

---

# Objectives

The objectives of this project were to:

- Configure a Microsoft Entra ID tenant
- Create and manage cloud user accounts
- Configure security groups
- Manage administrative roles
- Configure licensing
- Prepare identities for Hybrid Identity integration
- Establish centralized cloud identity administration

---

# Microsoft Entra ID Architecture

![Microsoft Entra ID Architecture](architecture.png)

---

# Environment

### Platform

- Microsoft Entra ID
- Microsoft Azure
- Microsoft Entra Admin Center

---

# Cloud User Management

Microsoft Entra ID was configured to centrally manage cloud identities and administrative accounts.

Administrative tasks included:

- Creating cloud users
- Managing passwords
- Assigning licenses
- Managing user properties
- Enabling administrative accounts

<img width="937" height="747" alt="image" src="https://github.com/user-attachments/assets/d3285b1f-4094-492a-bdd8-186148ae77e1" />


---

# Group Management

Security groups were created to simplify permission management and administrative delegation.

Examples include:

- VM Administrators
- Break Glass Administrators
- IT Administrators
- Security Administrators

### Screenshot

<img width="1244" height="842" alt="image" src="https://github.com/user-attachments/assets/51f73064-6ba2-450c-a14b-428a7868dad8" />

---

# Administrative Roles

Administrative roles were assigned using Microsoft Entra's built-in role-based administration model.

Examples include:

- Global Administrator
- User Administrator
- Security Administrator

Role assignments follow the principle of least privilege by granting only the permissions required for administrative tasks.

<img width="1260" height="372" alt="image" src="https://github.com/user-attachments/assets/18d22899-deda-41ba-8826-de17355c6c05" />


---

# Licensing

Microsoft Entra licensing enables advanced identity and security capabilities.

Configured licensing supports features such as:

- Conditional Access
- Identity Protection
- Identity Governance
- Azure RBAC integration

<img width="800" height="471" alt="image" src="https://github.com/user-attachments/assets/1094421b-c8a0-4b7c-b658-f45123f7e413" />


---

# Authentication

Microsoft Entra ID provides centralized authentication for cloud resources.

Authentication capabilities include:

- Username and password authentication
- Multi-Factor Authentication (MFA)
- Single Sign-On (SSO)
- Risk-based authentication
- Conditional Access integration

---

# Security Principles

This implementation demonstrates:

- Cloud Identity Management
- Least Privilege
- Zero Trust
- Role-Based Administration
- Centralized Authentication
- Identity Lifecycle Management

---

# Technologies Used

- Microsoft Entra ID
- Microsoft Azure
- Microsoft Entra Admin Center
- Azure Portal

---

# Skills Demonstrated

- Cloud Identity Administration
- User Management
- Group Management
- Administrative Role Assignment
- Microsoft Entra Administration
- Identity Security

---

# Outcome

A centralized Microsoft Entra ID environment was deployed to provide cloud identity management for the Contoso Enterprise Identity Platform.

This implementation establishes the cloud identity foundation used throughout later projects, including Hybrid Identity, Azure RBAC, Conditional Access, Identity Governance, and enterprise application management.
