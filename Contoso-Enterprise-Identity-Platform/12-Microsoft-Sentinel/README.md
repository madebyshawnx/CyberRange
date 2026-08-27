# Microsoft Sentinel Integration

## Overview

Configured Microsoft Sentinel to ingest Azure Activity Logs by connecting the Azure subscription to a Log Analytics workspace. This enables centralized monitoring of management-plane operations such as resource creation, policy changes, RBAC modifications, subscription events, and administrative actions.

---

## Objectives

- Connect Azure Activity logs to Microsoft Sentinel
- Configure Azure Monitor Diagnostic Settings
- Forward subscription activity into a Log Analytics Workspace
- Validate successful log ingestion using Kusto Query Language (KQL)
- Prepare Microsoft Sentinel for threat hunting, analytics rules, and incident investigations

---

## Technologies Used

- Microsoft Sentinel
- Azure Monitor
- Azure Activity Logs
- Azure Monitor Diagnostic Settings
- Log Analytics Workspace
- Kusto Query Language (KQL)

---

## Configuration Steps

### Step 1 – Deploy Microsoft Sentinel

Microsoft Sentinel was deployed using the **ACHATECH-Sentinel-LAW** Log Analytics workspace, providing a centralized SIEM platform for collecting, analyzing, and investigating Azure security telemetry.

**Figure 1.** Microsoft Sentinel successfully deployed on the Log Analytics workspace.

<img width="1541" height="791" alt="image" src="https://github.com/user-attachments/assets/89d3bbcd-5e73-4d3b-b997-1cfb91903294" />

---

### Step 2 – Configure the Azure Activity Connector

The Azure Activity connector was configured within Microsoft Sentinel to collect subscription-level administrative events. This connector enables Microsoft Sentinel to ingest Azure Activity Logs for security monitoring and investigation.

**Figure 2.** Azure Activity connector connected to the Azure subscription.

<img width="417" height="860" alt="image" src="https://github.com/user-attachments/assets/4c94d8e0-cd1d-412e-8bf9-2708594b271e" />

---

### Step 3 – Configure Subscription Diagnostic Settings

Subscription-level Diagnostic Settings were configured to forward Azure Activity Logs to the **ACHATECH-Sentinel-LAW** Log Analytics workspace.

The following log categories were enabled:

- Administrative
- Security
- Service Health
- Alert
- Recommendation
- Policy
- Autoscale
- Resource Health

**Figure 3.** Azure Activity diagnostic settings configured to forward subscription logs into Microsoft Sentinel.

<img width="962" height="613" alt="image" src="https://github.com/user-attachments/assets/124c0376-60d7-4499-9949-f6efc6c13951" />

---

### Step 4 – Validate Policy Compliance

Azure Policy was used to verify that the subscription met Microsoft's logging requirements. After the diagnostic settings were configured, the policy successfully re-evaluated and reported a **Compliant** state.

**Figure 4.** Azure Policy confirming successful compliance after configuring Azure Activity logging.

<img width="751" height="376" alt="image" src="https://github.com/user-attachments/assets/0b99ed29-4f04-43b4-9117-2ef75f09e7b5" />

---

### Step 5 – Verify Log Ingestion

To verify successful ingestion, Azure Activity Logs were queried using Kusto Query Language (KQL).

```kusto
AzureActivity
| sort by TimeGenerated desc
```

The query returned Azure administrative events, confirming that Microsoft Sentinel was successfully receiving subscription activity logs.

**Figure 5.** Azure Activity events successfully ingested into Microsoft Sentinel.

<img width="751" height="376" alt="image" src="https://github.com/user-attachments/assets/0b99ed29-4f04-43b4-9117-2ef75f09e7b5" />

<img width="1200" height="553" alt="image" src="https://github.com/user-attachments/assets/597ecf5b-d608-457a-b295-d322194abc61" />

---

## Security Value

Azure Activity Logs provide visibility into management-plane operations including:

- Resource deployments
- Resource deletions
- RBAC role assignments
- Azure Policy changes
- Subscription configuration changes
- Administrative operations

This telemetry provides the foundation for threat hunting, analytics rules, automation, and incident response within Microsoft Sentinel.

---

## Skills Demonstrated

- Microsoft Sentinel
- Azure Activity Connector
- Azure Monitor
- Azure Monitor Diagnostic Settings
- Azure Activity Logs
- Log Analytics Workspace
- Kusto Query Language (KQL)
- Cloud Security Monitoring
- Security Operations
