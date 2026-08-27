# Hybrid Identity — Active Directory to Microsoft Entra ID

## Overview

Implemented a hybrid identity architecture connecting the on-premises
`achatech.local` Active Directory environment to Microsoft Entra ID.

The project initially evaluated Microsoft Entra Cloud Sync as the
synchronization solution. After persistent provisioning-agent timeouts,
Microsoft Entra Connect Sync was deployed as the synchronization platform.

The final environment successfully synchronizes on-premises identities
to Microsoft Entra ID using Password Hash Synchronization (PHS).

---

## Architecture

On-Premises Active Directory
        |
        v
Microsoft Entra Connect Sync
        |
        v
Microsoft Entra ID

### Environment

- Windows Server 2022
- Active Directory Domain Services
- `achatech.local`
- Microsoft Entra ID
- Microsoft Entra Connect Sync
- Password Hash Synchronization
- OU-based synchronization scoping

---

## 1. On-Premises Identity Source

Active Directory serves as the authoritative identity source for the
hybrid environment.

The domain contains organizational units representing departments such
as Engineering, Finance, HR, IT, Sales, and other enterprise resources.

Users are created and managed within Active Directory before being
synchronized to Microsoft Entra ID.

### Evidence — Active Directory Structure

<img width="958" height="1006" alt="image" src="https://github.com/user-attachments/assets/5db8d4dd-2a38-4c47-b45d-54edb4c7ba83" />


**What this demonstrates:**  
An established on-premises Active Directory identity source with
organizational separation of enterprise identities.

---

## 2. Microsoft Entra Cloud Sync Evaluation

Microsoft Entra Cloud Sync was initially evaluated as the synchronization
architecture.

The Microsoft Entra Provisioning Agent was successfully:

- Installed on the Windows Server environment
- Registered with the Microsoft Entra tenant
- Configured to access `achatech.local`
- Configured with a group Managed Service Account (gMSA)
- Detected as active by Microsoft Entra

Provisioning attempts, however, repeatedly failed with:

`HybridIdentityServiceAgentTimeout`

and:

`ConnectorTimeout`

The Microsoft Entra provisioning service reported that it did not receive
a timely response from the on-premises provisioning environment.

### Troubleshooting Performed

The investigation included:

- Verified provisioning-agent service status
- Verified agent registration with Microsoft Entra
- Validated the provisioning-agent gMSA
- Verified Active Directory/domain-controller accessibility
- Verified outbound HTTPS connectivity over TCP 443
- Verified outbound HTTP connectivity over TCP 80
- Successfully connected to Microsoft provisioning services over HTTPS
- Verified WinHTTP was configured for direct Internet access
- Reinstalled and reconfigured the provisioning agent
- Recreated the gMSA
- Recreated the Cloud Sync configuration

Despite these checks, the provisioning-agent timeout persisted.

### Evidence — Cloud Sync Failure

<img width="865" height="327" alt="image" src="https://github.com/user-attachments/assets/515d3726-c996-4b38-b10e-72c46b683679" />


**What this demonstrates:**  
Identification and investigation of a persistent hybrid identity
provisioning failure before selecting an alternative synchronization
architecture.

---

## 3. Microsoft Entra Connect Sync Deployment

To satisfy the hybrid identity requirement, Microsoft Entra Connect Sync
was deployed as the synchronization platform.

The configuration connected the `achatech.local` Active Directory forest
to the Microsoft Entra tenant.

### Synchronization Configuration

The deployment included:

- Password Hash Synchronization (PHS)
- Active Directory forest integration
- Microsoft Entra tenant integration
- Selected OU synchronization
- `ms-DS-ConsistencyGuid` source anchor
- User and group synchronization

OU filtering was used to control which portions of the Active Directory
environment participate in synchronization rather than synchronizing the
entire directory indiscriminately.

### Evidence — OU Filtering

<img width="957" height="978" alt="image" src="https://github.com/user-attachments/assets/3dfc2786-ca6b-4270-a508-7b322b7ab0dd" />

**What this demonstrates:**  
Controlled synchronization scope based on organizational requirements
rather than unrestricted directory synchronization.

---

## 4. Synchronization Validation

After configuration, Microsoft Entra Connect initiated the initial
synchronization cycle.

Synchronization Service Manager confirmed successful execution of the
identity synchronization pipeline.

The synchronization process completed:

- Full Import — **Success**
- Full Synchronization — **Success**
- Export — **Success**

### Evidence — Successful Synchronization Engine

<img width="956" height="981" alt="image" src="https://github.com/user-attachments/assets/f332b02c-414b-4447-a48a-f1e90ba3cd73" />


**What this demonstrates:**  
Successful processing of identities through the Microsoft Entra Connect
synchronization engine from import through cloud export.

---

## 5. Microsoft Entra ID Validation

The final validation was performed directly within Microsoft Entra ID.

On-premises Active Directory identities, including departmental users,
were successfully created/synchronized in the Microsoft Entra tenant.

This confirmed the complete identity synchronization path:

Active Directory → Entra Connect Sync → Microsoft Entra ID

### Evidence — Synchronized Identities

<img width="778" height="635" alt="image" src="https://github.com/user-attachments/assets/0e89de4c-2b92-4201-a2e1-02ee7a761f63" />


**What this demonstrates:**  
On-premises identities successfully reaching Microsoft Entra ID,
confirming that the hybrid identity architecture is operational.

---

## 6. Final Architecture

The completed hybrid identity environment uses Microsoft Entra Connect
Sync to bridge the on-premises Active Directory environment with
Microsoft Entra ID.

Final identity flow:

On-Premises Active Directory
        |
        |  Identity + Password Hash Synchronization
        v
Microsoft Entra Connect Sync
        |
        v
Microsoft Entra ID
        |
        +--> Conditional Access
        +--> Azure RBAC
        +--> Privileged Identity Management
        +--> Identity Governance
        +--> Enterprise Applications

This provides the identity foundation for implementing additional
Zero Trust and identity-security controls throughout the enterprise
environment.

---

## Key Skills Demonstrated

- Hybrid identity architecture
- Active Directory administration
- Microsoft Entra ID
- Microsoft Entra Connect Sync
- Password Hash Synchronization
- OU-based synchronization scoping
- Identity synchronization troubleshooting
- Provisioning-agent troubleshooting
- gMSA configuration and validation
- Synchronization monitoring and validation
- Hybrid identity migration/architecture decision-making

---

## Outcome

Successfully established hybrid identity between the on-premises
`achatech.local` Active Directory environment and Microsoft Entra ID.

After troubleshooting persistent Microsoft Entra Cloud Sync provisioning
timeouts, Microsoft Entra Connect Sync was implemented as the alternative
synchronization architecture.

The completed environment successfully imports, synchronizes, and exports
on-premises identities to Microsoft Entra ID, providing the hybrid identity
foundation required for subsequent access management, Conditional Access,
privileged access, and identity governance controls.
