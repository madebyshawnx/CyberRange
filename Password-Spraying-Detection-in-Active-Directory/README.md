# Password Spraying Detection in Active Directory

## Executive Summary

This lab demonstrates how password spraying activity can be identified in an Active Directory environment and how Account Lockout Policy responds to repeated authentication failures. A single incorrect password was used against multiple domain user accounts to simulate password spraying. Windows Security logs were then analyzed to investigate the resulting Kerberos authentication failures and account lockout events.

## Lab Environment

- Windows Server 2022 (DC01)
- Windows 10 Enterprise (SalesPC01)
- Active Directory Domain Services
- Group Policy
- Event Viewer

---

## Attack Overview

Password spraying is a password attack that attempts one common password against many user accounts to avoid triggering account lockout policies. In this lab, the same incorrect password was attempted against multiple Active Directory user accounts to generate Kerberos pre authentication failures. Additional authentication attempts against a single test account were then used to demonstrate how Active Directory enforces its Account Lockout Policy once the configured threshold is exceeded.

---

## Walkthrough

### 1. Configure Account Lockout Policy

<img width="653" height="611" alt="image" src="https://github.com/user-attachments/assets/06fd0339-7f07-41f0-b221-1c4caf93bfff" />


Configured the Default Domain Policy to lock accounts after three failed logon attempts.

---

### 2. Update Group Policy

<img width="651" height="612" alt="image" src="https://github.com/user-attachments/assets/bd48b189-df8e-4aff-9c4b-58d687444db2" />


Ran `gpupdate /force` on the domain-joined workstation to apply the updated policy.

---

### 3. Simulate Password Spraying

Attempted the same incorrect password against multiple domain user accounts.

Each authentication attempt generated Event ID 4771 (Kerberos pre authentication failed), allowing the activity to be observed in Windows Security logs.


---

### 4. Investigate Windows Events

<img width="958" height="748" alt="image" src="https://github.com/user-attachments/assets/dd9b0b28-bb01-41b8-a14c-ae35fcddadb2" />

<img width="1012" height="888" alt="image" src="https://github.com/user-attachments/assets/32a937be-ae43-445b-8d8b-b7913586700f" />

<img width="842" height="566" alt="image" src="https://github.com/user-attachments/assets/7cabb588-cb3e-4c51-92a2-c87d6f1ac406" />


Reviewed the Security log to analyze the authentication failures and account lockout events and verified the account lockout using Windows Security logs.

---

## Detection

- Event ID **4771** – Kerberos pre-authentication failed
- Event ID **4740** – User account locked out
- Multiple Kerberos pre authentication failures across multiple user accounts
- Sudden increase in authentication failures

---

## MITRE ATT&CK

- **T1110.003 – Password Spraying**

---

## Lessons Learned

- Password spraying attempts to avoid account lockout by distributing authentication attempts across multiple users.
- Kerberos pre authentication failures provide valuable telemetry for identifying suspicious authentication activity.
- Group Policy centrally enforces Account Lockout Policy across the domain.
- Event ID 4740 confirms when the account lockout threshold has been reached.

## Authentication Flow

During each authentication attempt, Windows processes credentials through the following sequence:

```text
Credentials Submitted
        ↓
LSASS
        ↓
Kerberos Authentication
        ↓
Active Directory
        ↓
Update User State
        ↓
Evaluate Account Lockout Policy
        ↓
Generate Security Event
        ↓
Respond to Client
```
