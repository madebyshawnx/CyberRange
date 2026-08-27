# Privileged Identity Management (PIM)

## Project Overview

Designed and implemented Microsoft Entra Privileged Identity Management (PIM) to enforce Just-in-Time (JIT) privileged access for administrative roles. Rather than granting permanent administrative privileges, privileged roles were assigned as **Eligible**, requiring administrators to activate access through Azure Multi-Factor Authentication (MFA) and provide business justification before receiving temporary elevated permissions. This implementation demonstrates modern Identity and Access Management (IAM) practices based on Zero Trust and the Principle of Least Privilege.

---

# Objectives

- Implement Microsoft Entra Privileged Identity Management (PIM)
- Configure Eligible role assignments
- Implement Just-in-Time (JIT) administration
- Require Azure MFA for privileged role activation
- Require business justification for privilege elevation
- Configure temporary administrative access
- Demonstrate least privilege administration
- Audit privileged access activity

---

# Environment

| Component | Technology |
|-----------|------------|
| Identity Platform | Microsoft Entra ID |
| Privileged Access | Microsoft Entra PIM |
| Administrative Role | User Administrator |
| Authentication | Azure MFA |
| Access Model | Just-in-Time (JIT) |
| Security Model | Least Privilege |
| Governance | Role Activation Policies |

---

# Architecture

> **Figure 1. Privileged Identity Management Architecture**

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/9cec1e27-b81c-4cfe-8989-f27549865295" />


This architecture demonstrates Microsoft Entra Privileged Identity Management controlling privileged access through eligible role assignments. Administrators must authenticate with Azure MFA and provide justification before temporarily activating administrative privileges. Once the activation period expires, elevated permissions are automatically removed.

---

# Project Walkthrough

---

## 1. Configure Eligible Role Assignment

Configured the **User Administrator** role as an **Eligible** assignment instead of granting permanent administrative privileges.

Eligible assignments allow administrators to activate privileged access only when required.

### Concepts Demonstrated

- Eligible Assignments
- Least Privilege
- Standing vs Just-in-Time Administration

📷 **Figure 2. Eligible Role Assignment**
<img width="1516" height="353" alt="image" src="https://github.com/user-attachments/assets/6a1acc62-e579-40eb-a4f9-6116bb3865d8" />

---

## 2. Configure Role Activation Policy

Configured activation requirements for the User Administrator role.

Settings included:

- Azure Multi-Factor Authentication (MFA)
- Business Justification
- Maximum Activation Duration (2 Hours)

This ensures privileged access is both verified and time-limited.

📷 **Figure 3. Role Activation Policy**

<img width="712" height="865" alt="image" src="https://github.com/user-attachments/assets/011ac1b1-bec0-487f-88bf-e0bfe1c0932d" />


---

## 3. Just-in-Time (JIT) Role Activation

Activated the User Administrator role using Microsoft Entra Privileged Identity Management.

During activation:

- Azure MFA verified administrator identity
- Business justification was required
- Temporary administrative privileges were granted
- Automatic expiration time was assigned

This demonstrates Microsoft's Just-in-Time administration model.

📷 **Figure 4. Role Activation Process**

<img width="733" height="880" alt="image" src="https://github.com/user-attachments/assets/d29cc55b-974a-4a49-98fb-9324144e798b" />

---

## 4. Temporary Privileged Access

Verified the activated User Administrator assignment within the PIM portal.

The role displayed:

- Active Assignment
- Activation Status
- Automatic Expiration Time

Unlike permanent administrators, elevated permissions are automatically revoked when the activation window expires.

📷 **Figure 5. Active Privileged Role**

<img width="1552" height="284" alt="image" src="https://github.com/user-attachments/assets/57ddcffb-092c-4cae-a246-b33a3f2c4c51" />


---

# Privileged Access Workflow

> **Figure 6. Just-in-Time Privileged Access Workflow**

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/23c7746c-cb63-4356-9980-7d1c732fd827" />


Privilege Elevation Process

1. Administrator receives an Eligible role assignment.
2. Administrator requests role activation.
3. Microsoft Entra ID requires Azure MFA.
4. Administrator provides business justification.
5. Microsoft Entra PIM temporarily activates the role.
6. Administrative tasks are performed.
7. Activation timer expires.
8. Elevated permissions are automatically removed.

---

# Key Concepts Learned

- Microsoft Entra Privileged Identity Management
- Just-in-Time (JIT) Administration
- Eligible Assignments
- Active Assignments
- Least Privilege
- Privileged Role Activation
- Azure Multi-Factor Authentication
- Business Justification
- Temporary Privileged Access
- Privileged Governance
- Standing Privileges
- Role Expiration

---

# Skills Demonstrated

- Microsoft Entra ID Administration
- Privileged Identity Management (PIM)
- Identity Governance
- Zero Trust Administration
- Just-in-Time Privileged Access
- Azure MFA Configuration
- Least Privilege Access Control
- Administrative Role Management
- Enterprise Identity Security

---

# Conclusion

This project demonstrates enterprise privileged access management using Microsoft Entra Privileged Identity Management (PIM). Administrative roles were configured as Eligible assignments, requiring Azure MFA and business justification before granting temporary privileged access. By replacing permanent administrator accounts with Just-in-Time (JIT) activation, the implementation reduces standing privileges, strengthens identity security, and aligns with Zero Trust security principles.

---

# Screenshot Checklist

| Figure | Screenshot |
|---------|------------|
| Figure 1 | Privileged Identity Management Architecture Diagram |
| Figure 2 | Eligible Role Assignment |
| Figure 3 | Role Activation Policy (MFA + Justification + 2 Hour Duration) |
| Figure 4 | PIM Role Activation Status |
| Figure 5 | Active Assignment with Expiration Time |
| Figure 6 | Just-in-Time Privileged Access Workflow Diagram |
