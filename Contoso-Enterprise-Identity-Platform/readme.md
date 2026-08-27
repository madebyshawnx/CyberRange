# (ACHATECH) Contoso Enterprise Identity Platform

## Overview

The Contoso Enterprise Identity Platform is a hands-on enterprise identity and access management (IAM) lab built to simulate a modern hybrid Microsoft environment.

This project demonstrates the deployment, administration, automation, and security of enterprise identities using Active Directory, Microsoft Entra ID, Azure RBAC, Conditional Access, PowerShell, Terraform, Microsoft Defender XDR, and Microsoft Sentinel.

Rather than isolated labs, each section builds upon the previous one to create a complete enterprise identity platform following Microsoft's Zero Trust security principles.

---

# Project Objectives

The primary goals of this project are to:

- Build an enterprise Active Directory environment
- Implement Hybrid Identity with Microsoft Entra Cloud Sync
- Secure identities using Microsoft Entra Conditional Access
- Automate Joiner-Mover-Leaver (JML) identity lifecycle management
- Implement Role-Based Access Control (RBAC)
- Deploy Infrastructure as Code using Terraform
- Demonstrate Identity Governance concepts
- Configure enterprise application identity management
- Investigate identity-based attacks
- Perform endpoint detection and response using Microsoft Defender XDR
- Conduct threat detection and investigation with Microsoft Sentinel

---

# Architecture


<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/906c4a62-f284-48bf-92d8-552811720f31" />

```text

```


---

# Technologies Used

### Identity

- Active Directory Domain Services
- Microsoft Entra ID
- Microsoft Entra Cloud Sync
- Microsoft Identity Governance

### Azure

- Azure RBAC
- Azure Resource Groups
- Azure Virtual Machines

### Security

- Microsoft Defender XDR
- Microsoft Sentinel
- Microsoft Entra Conditional Access
- Microsoft Identity Protection

### Automation

- PowerShell
- Terraform

### Infrastructure

- Windows Server
- Windows 10/11
- Azure Virtual Machines

---

# Repository Structure

| Folder | Description |
|----------|-------------|
| **00-Architecture** | Enterprise architecture diagrams and project overview |
| **01-Enterprise-Active-Directory** | Active Directory deployment, OUs, users, groups, Group Policy, and domain administration |
| **02-Microsoft-Entra-ID** | Microsoft Entra ID configuration and identity management |
| **03-Hybrid-Identity** | Hybrid Identity implementation using Microsoft Entra Cloud Sync |
| **04-Azure-RBAC** | Azure Role-Based Access Control implementation using least privilege |
| **05-Conditional-Access** | Enterprise Conditional Access policies implementing Zero Trust authentication |
| **06-Privileged-Identity-Management** | Administrative identity protection and privileged access management concepts |
| **07-Identity-Governance** | Identity Governance, access reviews, lifecycle management, and compliance |
| **08-Enterprise-Applications** | Enterprise application integration and Single Sign-On configuration |
| **09-PowerShell-Automation** | Joiner-Mover-Leaver (JML) automation using PowerShell |
| **10-Identity-Attack-Lab** | (COMING SOON) Identity attack simulations and detection techniques |
| **11-Microsoft-Defender-XDR** | Endpoint detection, investigation, and response workflows |
| **12-Microsoft-Sentinel** | SIEM deployment, KQL queries, analytics, and incident investigation |



---

# Skills Demonstrated

## Identity & Access Management

- Identity Administration
- Hybrid Identity
- Authentication
- Authorization
- Identity Lifecycle Management
- Identity Governance

## Security

- Zero Trust
- Conditional Access
- Identity Protection
- Endpoint Detection and Response
- Threat Hunting
- Incident Investigation

## Cloud

- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Cloud Identity

## Automation

- PowerShell Scripting
- Terraform
- Infrastructure as Code (IaC)

---

# Key Features

- Enterprise Active Directory deployment
- Hybrid identity synchronization
- Automated user provisioning (Joiner-Mover-Leaver)
- Microsoft Entra Conditional Access
- Azure RBAC implementation
- Identity Governance
- Enterprise application integration
- Endpoint detection with Microsoft Defender XDR
- Threat hunting with Microsoft Sentinel
- Zero Trust identity architecture

---

# Learning Outcomes

Through this project, I gained hands-on experience with:

- Designing enterprise identity architectures
- Managing hybrid identity environments
- Automating identity lifecycle processes
- Implementing least-privilege access controls
- Deploying risk-based authentication policies
- Investigating identity-related security events
- Securing Microsoft cloud identities using Zero Trust principles

---

# Future Enhancements

Potential future enhancements include:

- Privileged Identity Management (PIM) implementation
- Microsoft Intune device compliance
- Access Reviews automation
- Lifecycle Workflows
- SCIM provisioning
- External Identities (B2B/B2C)
- Identity Protection risk policies
- Passwordless authentication (FIDO2 & Windows Hello for Business)

---

# Author

**Ti Acha**

Cybersecurity | Identity & Access Management | Cloud Security

---

> **Disclaimer:** This repository was created for educational and portfolio purposes to demonstrate enterprise identity and access management concepts using Microsoft technologies.
