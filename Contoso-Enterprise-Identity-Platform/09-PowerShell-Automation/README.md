# PowerShell Automation

## Overview

This module automates identity lifecycle reconciliation between authoritative HR data and Active Directory

The PowerShell engine evaluates the desired identity state defined by HR against the current Active Directory state and automatically reconciles identity attributes and department-based access.

The workflow supports **Joiner, Mover, and Leaver (JML)** lifecycle operations while maintaining a timestamped audit trail.

---

## Architecture

```text
HR Employee Data (CSV)
        │
        ▼
PowerShell Reconciliation Engine
        │
        ├── Identity Resolution
        ├── Attribute Reconciliation
        ├── Manager Validation
        ├── Access Reconciliation
        └── Leaver Detection
        │
        ▼
Active Directory
        │
        ├── User Attributes
        ├── Security Groups
        └── Account Status
        │
        ▼
Timestamped Audit Log
```

---

## Identity Lifecycle Workflows

### Joiner / Active Employee

For active employees, the automation validates the corresponding AD identity and reconciles:

- Employee ID
- Department
- Job title
- Office/location
- Manager
- Department-based security group membership

### Mover

When HR changes an employee's department, the engine detects access drift and reconciles the account.

**Test scenario:**

```text
Mike Davis

HR Department: Sales → Marketing

GG_Sales     → Removed
GG_Marketing → Granted
Department   → Updated to Marketing
Manager      → Preserved
```

Only managed department groups are evaluated for removal, reducing the risk of unintentionally modifying unrelated access.

### Leaver

A populated HR `EndDate` that has been reached triggers the offboarding workflow.

**Test scenario:**

```text
Daniel Moore

EndDate reached
      ↓
Leaver detected
      ↓
AD account disabled
      ↓
Managed department access removed
      ↓
Normal provisioning skipped
      ↓
Action recorded in audit log
```

---

## Desired-State Reconciliation

The automation is designed to be **idempotent**.

Once Active Directory matches the HR-defined desired state, subsequent executions detect that no remediation is required rather than repeatedly modifying the account.

```text
HR Desired State
       │
       ▼
Compare with AD
       │
   ┌───┴───┐
   │       │
 Match    Drift
   │       │
 No       Reconcile
Change     AD
```

This behavior was validated for both mover and leaver workflows.

---

## Security Controls

- HR-driven identity reconciliation
- Duplicate identity detection
- Missing-account handling
- Manager validation
- Self-manager prevention
- Department-based access enforcement
- Obsolete access revocation
- Automated access provisioning
- Leaver detection
- Automated account disabling
- Managed-access removal
- Error handling
- Timestamped audit logging
- Idempotent execution

---

## Execution Evidence

The final reconciliation run processed all 10 employee identities after mover and leaver remediation.

The final audit state demonstrates successful access validation, manager reconciliation, completed offboarding, and convergence to the HR-defined desired state.

<img width="757" height="861" alt="image" src="https://github.com/user-attachments/assets/8a5adf7e-ac23-456c-be9e-a74152df5937" />


The final state confirms that Active Directory converged to the HR-defined desired state with no unnecessary access changes.

---

## Technologies

- PowerShell
- Active Directory Domain Services
- ActiveDirectory PowerShell Module
- Windows Server
- CSV-based HR identity data
- Active Directory security groups

---

## Skills Demonstrated

**Identity & Access Management:** Joiner-Mover-Leaver lifecycle management, identity reconciliation, access provisioning/deprovisioning, group-based access control, and offboarding.

**Automation Engineering:** PowerShell scripting, desired-state reconciliation, idempotency, validation, exception handling, and audit logging.

**Security Engineering:** Least privilege, access governance, privilege-creep prevention, automated deprovisioning, and identity lifecycle controls.

---

## Key Outcome

Built an HR-driven identity reconciliation engine that automatically detects and remediates Active Directory identity and access drift while providing auditable, idempotent lifecycle enforcement.
