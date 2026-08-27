# Conditional Access


## Overview

This project implements enterprise Conditional Access policies within the ACHATECH hybrid identity environment to strengthen authentication security and reduce identity-based attack risks.

Following the successful deployment of Microsoft Entra ID, Hybrid Identity, and Azure Role-Based Access Control (RBAC), Conditional Access introduces policy-based authentication controls that evaluate users, devices, locations, and sign-in risk before granting access.

The implementation follows Microsoft's Zero Trust identity principles by enforcing least privilege, strong authentication, and risk-based access policies.

<img width="809" height="630" alt="image" src="https://github.com/user-attachments/assets/6338bdfe-efc0-4027-8dcc-6067d4475880" />



---

# Architecture

The Conditional Access workflow implemented in this project is shown below.

```text
On-Premises Active Directory
        ↓
Microsoft Entra Cloud Sync
        ↓
Microsoft Entra ID
        ↓
Conditional Access
        ↓
Authentication Evaluation
        ↓
Azure Resources
```

Authentication decisions are no longer based solely on user credentials. Microsoft Entra evaluates contextual signals before access is granted.

These signals include:

- User identity
- Administrative role
- Client application
- Device compliance
- Sign-in risk
- Geographic location

---

# Objectives

The objective of this project was to implement enterprise authentication controls that reduce the likelihood of credential theft, account compromise, and unauthorized administrative access.

The Conditional Access strategy was designed to:

- Require Multi-Factor Authentication (MFA)
- Block insecure legacy authentication
- Protect privileged administrators
- Block risky sign-ins
- Restrict administrative access to compliant devices
- Protect against malicious sign-in locations
- Maintain emergency administrative access through a Break Glass account

---

# Environment

Platform

- Microsoft Entra ID Premium P2
- Microsoft Entra Conditional Access
- Hybrid Identity (Cloud Sync)
- Azure Resource Manager
- Azure RBAC

Identity Source

- On-premises Active Directory
- Microsoft Entra Cloud Sync

Deployment Method

- Microsoft Entra Admin Center

---

# Emergency Access Account

Before enforcing Conditional Access, a dedicated Break Glass Administrator account was created.

Purpose:

- Prevent tenant lockout
- Allow emergency administrative access
- Recover from Conditional Access misconfiguration

The Break Glass account was excluded from every Conditional Access policy.

This follows Microsoft's recommended emergency access account design.

---

# Conditional Access Policies

## CA-001 | Require MFA for Privileged Administrators

Purpose

Protect privileged administrative accounts by requiring Multi-Factor Authentication.

Configuration

Users

- Global Administrator

Excluded

- Break Glass Administrator

Resources

- All cloud resources

Grant Controls

- Require MFA

Policy State

- Report-only

---

## CA-002 | Block Legacy Authentication

Purpose

Prevent authentication using protocols that do not support modern authentication or MFA.

Examples include:

- Exchange ActiveSync
- Basic Authentication clients
- Other legacy authentication protocols

Users

- All users

Excluded

- Break Glass Administrator

Grant

- Block Access

Policy State

- Report-only

---

## CA-003 | Require MFA for All Users

Purpose

Ensure every user authenticates using Multi-Factor Authentication.

Users

- All users

Excluded

- Break Glass Administrator

Grant

- Require MFA

Policy State

- Report-only

---

## CA-004 | Block Sign-ins from High-Risk Countries

Purpose

Reduce unauthorized access attempts originating from selected geographic regions.

Users

- All users

Excluded

- Break Glass Administrator

Condition

- Named Locations

Grant

- Block Access

Policy State

- Report-only

---

## CA-005 | Block High Sign-In Risk

Purpose

Block authentication attempts that Microsoft Identity Protection identifies as high risk.

Users

- All users

Excluded

- Break Glass Administrator

Condition

- Sign-In Risk = High

Grant

- Block Access

Policy State

- Report-only

---

## CA-006 | Require Compliant Device for Administrators

Purpose

Limit privileged administrative access to trusted and compliant devices.

Users

- Administrative roles

Excluded

- Break Glass Administrator

Grant

- Require compliant device

Policy State

- Report-only

---

# Policy Validation

Microsoft Entra Conditional Access "What If" analysis was used to validate policy evaluation before enforcement.

Testing confirmed:

- Administrator MFA policy successfully evaluated.
- Targeted identities matched policy conditions.
- Grant controls required MFA.
- Break Glass exclusions functioned correctly.
- Policies evaluated successfully while operating in Report-only mode.

Using Report-only mode allows organizations to evaluate policy impact without interrupting production authentication.

---

# Security Principles Demonstrated

## Zero Trust

Authentication decisions are continuously evaluated rather than automatically trusted.

---

## Least Privilege

Administrative access receives stronger authentication requirements than standard users.

---

## Defense in Depth

Multiple Conditional Access policies work together to mitigate different attack vectors.

Examples include:

- Credential theft
- Legacy authentication abuse
- High-risk sign-ins
- Geographic attacks
- Device compromise

---

## Identity Protection

Microsoft Entra Identity Protection signals are incorporated into authentication decisions.

---

## Business Continuity

Emergency administrative access is preserved through the Break Glass account.

---

# Outcome

The ACHATECH enterprise identity platform now enforces layered Conditional Access policies across the Microsoft Entra environment.

Authentication decisions are evaluated using user identity, device trust, geographic location, administrative privilege, and Microsoft Identity Protection risk signals before access is granted.

The resulting implementation demonstrates enterprise identity security aligned with Microsoft's Zero Trust architecture and modern Conditional Access best practices.
