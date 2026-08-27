# Identity Governance


## Contoso Identity & Access Management Standard

## Purpose

This standard establishes consistent requirements for creating, managing, modifying, and removing identities within Contoso's enterprise environment.

The objective is to reduce inconsistent administrative practices, prevent excessive access, enforce least privilege, and establish a standardized identity lifecycle.

---

## 1. Identity Source of Truth

Human Resources (HR) is the authoritative source for workforce identity information.

Required identity attributes include:

- Employee ID
- First name
- Last name
- Department
- Job title
- Location
- Manager
- Employment type
- Start date
- End date

Changes to authoritative HR information must be reflected in enterprise identity systems through approved lifecycle processes.

---

## 2. Account Naming Standard

Standard employee accounts must follow a consistent naming convention.

**Format:**

First name + last initial

Example:

John Doe → `John.d`

Account names must be unique. Naming conflicts must be reviewed before account creation.

---

## 3. Role-Based Access Control

Access must be assigned through approved security groups rather than direct user permissions whenever technically feasible.

Standard access model:

User
↓
Department / Job Role
↓
Security Group
↓
Required Permissions
↓
Enterprise Resource

Department membership determines baseline access.

Examples:

Sales → GG_Sales  
Finance → GG_Finance  
HR → GG_HR  
IT → GG_IT  
Marketing → GG_Marketing

Additional privileged or application-specific access requires separate authorization.

---

## 4. Least Privilege

Users must receive only the access necessary to perform their assigned responsibilities.

Administrators must:

- Avoid unnecessary permissions.
- Prefer group-based access over direct assignments.
- Remove obsolete access following role changes.
- Separate standard and privileged administrative access.
- Review elevated access independently from baseline department access.

Access must not be retained solely because it was previously granted.

---

## 5. Joiner Process

New workforce identities must originate from approved HR records.

Before access is provisioned:

1. Required identity attributes must be present.
2. The identity must be uniquely resolved.
3. Department and job information must be validated.
4. Baseline access must be assigned through approved security groups.
5. Manager information must be validated where applicable.

---

## 6. Mover Process

Changes in department, role, or responsibility require access reconciliation.

The identity management process must:

1. Determine the employee's new required access.
2. Identify obsolete managed access.
3. Revoke access no longer required.
4. Provision newly required access.
5. Update applicable identity attributes.
6. Record lifecycle actions for auditing.

This process prevents privilege accumulation following organizational changes.

---

## 7. Leaver Process

When an employee reaches an approved termination/end date:

1. The identity must be identified as a leaver.
2. Interactive account access must be disabled.
3. Managed department access must be removed.
4. Normal provisioning must stop.
5. Offboarding actions must be recorded.

Accounts must not remain active solely because manual deprovisioning was overlooked.

---

## 8. Access Approval

Baseline department access may be provisioned according to approved RBAC mappings.

Access outside the employee's baseline role requires authorization from the appropriate resource owner, manager, or designated approver.

Privileged administrative access requires additional approval and must not be automatically inherited from standard department membership.

---

## 9. Administrative Accounts

Privileged administration should use dedicated administrative identities where practical.

Standard workforce accounts should not receive permanent administrative privileges solely for convenience.

Administrative permissions must follow least-privilege principles and be limited to the required scope.

---

## 10. Audit and Accountability

Identity lifecycle operations must generate sufficient records to support investigation and review.

Audit records should identify:

- Employee/identity
- Lifecycle operation
- Access affected
- Result
- Timestamp
- Errors or exceptions

Automated identity processes should be designed to avoid unnecessary repeated modifications when the desired state has already been reached.

---

## 11. Enforcement

Contoso identity systems and automation should enforce this standard wherever technically feasible.

Exceptions must be documented and reviewed rather than implemented as undocumented permanent access.
