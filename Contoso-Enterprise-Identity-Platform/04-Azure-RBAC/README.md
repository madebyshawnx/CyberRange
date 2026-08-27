# Azure Role-Based Access Control (RBAC)

## Overview

This project implements group-based Azure Role-Based Access Control (RBAC) for the ACHATECH enterprise environment.

The objective was to extend the hybrid identity architecture established with Microsoft Entra Connect Sync into Azure resource authorization.

Rather than creating separate cloud identities, existing users synchronized from the on-premises `achatech.local` Active Directory environment are used for Azure access.

Terraform is used to manage Azure resource groups, Microsoft Entra security groups, group membership, and Azure RBAC role assignments.

---

## Architecture

The authorization flow implemented in this project is:

```text
On-Premises Active Directory
        ↓
Microsoft Entra Connect Sync
        ↓
Microsoft Entra ID Hybrid Identity
        ↓
Microsoft Entra Security Group
        ↓
Azure RBAC Role Assignment
        ↓
Azure Resource Group
```

Example:

```text
Chris Walker
        ↓
ACHATECH-VM-Admins
        ↓
Virtual Machine Contributor
        ↓
ACHATECH-Production
```

This architecture separates identity management from authorization and allows permissions to be assigned through security groups rather than directly to individual users.

---

## Hybrid Identity Integration

Chris Walker was originally created in the on-premises `achatech.local` Active Directory environment and synchronized into Microsoft Entra ID using Microsoft Entra Connect Sync.

The synchronized identity was verified in Microsoft Entra ID.

Key attributes included:

- On-premises sync enabled: `Yes`
- On-premises domain: `achatech.local`
- On-premises account: `Chris.w`
- Microsoft Entra UPN: `Chris.w@tajacha3outlook.onmicrosoft.com`
- Distinguished Name: `CN=Chris Walker,OU=Sales,DC=achatech,DC=local`

### Evidence — Hybrid Identity

<img width="570" height="293" alt="image" src="https://github.com/user-attachments/assets/a99bb8fd-dc0e-437e-aee3-7451a4e78476" />


This confirms that Azure authorization is being applied to an identity originating from the on-premises Active Directory environment rather than to a duplicate cloud-created account.

---

## Terraform Identity Lookup

Terraform does not create the hybrid user.

Instead, the existing synchronized Microsoft Entra identity is retrieved using a data source:

```hcl
data "azuread_user" "chris_walker" {
  user_principal_name = "Chris.w@tajacha3outlook.onmicrosoft.com"
}
```

Using a data source allows Terraform to reference the existing identity's object ID without attempting to manage the lifecycle of the synchronized user.

---

## RBAC Security Groups

Microsoft Entra security groups are used as the authorization layer between users and Azure resources.

The RBAC groups include:

- `ACHATECH-VM-Admins`
- `ACHATECH-Network-Admins`
- `ACHATECH-Storage-Admins`
- `ACHATECH-Readers`
- `ACHATECH-Security-Readers`

Example Terraform configuration:

```hcl
locals {
  rbac_groups = [
    "VM-Admins",
    "Network-Admins",
    "Storage-Admins",
    "Readers",
    "Security-Readers"
  ]
}

resource "azuread_group" "rbac" {
  for_each = toset(local.rbac_groups)

  display_name     = "ACHATECH-${each.value}"
  security_enabled = true
}
```

---

## Hybrid User Group Assignment

Chris Walker was assigned to the `ACHATECH-VM-Admins` security group using Terraform.

```hcl
resource "azuread_group_member" "chris_vm_admin" {
  group_object_id  = azuread_group.rbac["VM-Admins"].object_id
  member_object_id = data.azuread_user.chris_walker.object_id
}
```

Terraform therefore manages the authorization relationship while Microsoft Entra Connect remains responsible for synchronizing the underlying identity.

### Evidence — Security Group Membership

<img width="900" height="597" alt="image" src="https://github.com/user-attachments/assets/da48ea41-8aad-4bbc-9e82-2b371dcb8e8a" />


The Microsoft Entra admin center confirms that Chris Walker is a direct member of `ACHATECH-VM-Admins`.

---

## Azure Resource Scope

Terraform manages multiple Azure resource groups representing different enterprise environments:

- `ACHATECH-Production`
- `ACHATECH-Development`
- `ACHATECH-Testing`
- `ACHATECH-Security`
- `ACHATECH-SharedServices`

Resource groups provide scopes at which Azure RBAC permissions can be assigned.

---

## Azure RBAC Assignment

The `ACHATECH-VM-Admins` group is assigned the built-in Azure role:

**Virtual Machine Contributor**

The assignment is scoped to:

**ACHATECH-Production**

Terraform configuration:

```hcl
resource "azurerm_role_assignment" "vm_admins" {
  scope = azurerm_resource_group.resource_groups["Production"].id

  role_definition_name = "Virtual Machine Contributor"

  principal_id = azuread_group.rbac["VM-Admins"].object_id
}
```

This means members of `ACHATECH-VM-Admins` receive VM management permissions within the Production resource group through group membership.

### Evidence — Azure RBAC Assignment

<img width="621" height="607" alt="image" src="https://github.com/user-attachments/assets/27eb8e77-0d82-4eb8-a9dc-c9e7ec33ecbc" />



The Azure IAM configuration confirms:

`ACHATECH-VM-Admins` → `Virtual Machine Contributor` → `ACHATECH-Production`

---

## Effective Access Validation

Configuring a role assignment does not by itself prove that the intended user receives the permission.

Azure IAM **Check access** was therefore used to validate Chris Walker's effective permissions.

Azure confirmed:

- User: `Chris Walker`
- Role: `Virtual Machine Contributor`
- Scope: `ACHATECH-Production`
- Group assignment: `ACHATECH-VM-Admins`

### Evidence — Effective Access

<img width="900" height="590" alt="image" src="https://github.com/user-attachments/assets/557f232e-166f-4c5f-9ebc-034cdf5adfa4" />


This validates the complete authorization chain:

```text
On-Premises Active Directory
        ↓
Microsoft Entra Connect
        ↓
Chris Walker
        ↓
ACHATECH-VM-Admins
        ↓
Virtual Machine Contributor
        ↓
ACHATECH-Production
```

---

## Least-Privilege Validation

A second synchronized user, Sarah Johnson, was used as a negative control.

Sarah was not assigned to the `ACHATECH-VM-Admins` group.

Azure IAM Check Access returned:

```text
Role assignments: 0
```

for Sarah at the `ACHATECH-Production` scope.

<img width="961" height="437" alt="image" src="https://github.com/user-attachments/assets/beb5f823-728f-4941-8350-9c16622a6878" />


This demonstrates that synchronization into Microsoft Entra ID does not automatically provide access to Azure resources.

Access is granted according to group membership and assigned Azure RBAC roles.

---

## Terraform Validation

Before deployment, the Terraform configuration was formatted and validated:

```powershell
terraform fmt
terraform validate
```

Terraform returned:

```text
Success! The configuration is valid.
```

A deployment plan was then reviewed before applying changes:

```powershell
terraform plan
```

The plan showed:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

The only new resource was Chris Walker's membership in the `ACHATECH-VM-Admins` group.

This ensured that the existing identity infrastructure and RBAC assignments would not be unintentionally modified or destroyed.

---

## Security Principles Demonstrated

### Group-Based Authorization

Permissions are assigned to security groups rather than directly to individual users.

### Least Privilege

Users receive only the permissions required for their assigned responsibilities.

### Separation of Identity and Authorization

Microsoft Entra Connect synchronizes identities while Azure RBAC controls access to Azure resources.

### Scoped Access

The Virtual Machine Contributor role is scoped to the Production resource group rather than granting subscription-wide access.

### Infrastructure as Code

Terraform provides repeatable and auditable deployment of groups, memberships, Azure resource groups, and RBAC assignments.

### Access Validation

Effective permissions were independently verified through Azure IAM rather than assuming that the configuration produced the intended authorization.

---

## Outcome

The project successfully extended the ACHATECH hybrid identity environment into Azure authorization.

An on-premises Active Directory user was synchronized to Microsoft Entra ID, referenced by Terraform, assigned to an RBAC security group, and granted scoped Azure permissions through group-based role assignment.

Effective access testing confirmed that the authorized user inherited the expected permissions, while a user outside the privileged group received no role assignment at the same scope.

The resulting model demonstrates an end-to-end hybrid identity and cloud authorization workflow:

**On-Prem AD → Entra ID → Security Groups → Azure RBAC → Azure Resources**
