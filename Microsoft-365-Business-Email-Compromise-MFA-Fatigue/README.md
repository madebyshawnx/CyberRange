
# Microsoft 365 Business Email Compromise (MFA Fatigue)

## Platforms and Languages Leveraged

- Microsoft Sentinel
- Microsoft Entra ID (Azure AD)
- Microsoft 365
- Microsoft Defender XDR
- Kusto Query Language (KQL)

---

# Scenario

A finance employee reported receiving multiple unexpected Microsoft Authenticator push notifications before losing access to their Microsoft 365 account. Shortly afterward, another employee received an email requesting updated banking details for an invoice.

Management suspected a Business Email Compromise (BEC) attack resulting from an MFA fatigue campaign. The objective of this investigation was to determine how the attacker gained access, identify any persistence mechanisms, determine the scope of the compromise, and recommend containment actions.

---

## High-Level Investigation Plan

- Review **SigninLogs** for repeated MFA requests and successful authentication.
- Identify the attacker's source IP address.
- Review **CloudAppEvents** for malicious post-authentication activity.
- Inspect Outlook inbox rules for persistence.
- Review **EmailEvents** for Business Email Compromise activity.
- Determine whether additional Microsoft 365 resources were accessed.
- Correlate activity using Azure AD Session IDs.
- Map observed techniques to the MITRE ATT&CK framework.
- Recommend immediate containment actions.

---

# Steps Taken

## 1. Investigated Sign-in Activity

The investigation began by reviewing Azure AD sign-in logs during the suspected compromise window. Multiple authentication attempts requiring strong authentication were observed before a successful login occurred.

**Query used**

```kusto
SigninLogs
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| order by TimeGenerated asc
| where ResultDescription contains "strong authentication is required"
| where AuthenticationDetails contains "Mobile"
| project-away OperationName, OperationVersion, Category
```

### Findings

- Multiple Microsoft Authenticator push notifications were generated.
- Result Type **50074** ("Strong authentication is required") appeared repeatedly.
- The authentication requests targeted the user's registered mobile device.

The repeated MFA prompts strongly indicated an **MFA fatigue attack**, a technique where attackers continuously send push notifications until the victim eventually approves one.

<img width="1672" height="338" alt="image" src="https://github.com/user-attachments/assets/d131965e-fc90-4061-bc83-60d7232e1d30" />


---

## 2. Identified the Attacker's IP Address

The user's successful sign-ins were reviewed to compare normal authentication activity against the suspicious login.

**Query used**

```kusto
SigninLogs
| where TimeGenerated between (datetime(2026-02-25 14:00:00) .. datetime(2026-02-25 23:00:00))
| where UserPrincipalName == "m.smith@lognpacific.org"
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, IPAddress, Location, AppDisplayName
| order by TimeGenerated asc
```

### Findings

Two successful sign-in locations were identified.

**Legitimate User IP**

```
172.175.65.103
```

**Attacker IP**

```
205.147.16.190
```

The successful authentication from **205.147.16.190** occurred immediately after the repeated MFA prompts, confirming the attacker's point of entry.

<img width="1073" height="613" alt="image" src="https://github.com/user-attachments/assets/50fe6373-d0ca-4cdb-9ea6-22030b69e48e" />


---

## 3. Reviewed Authentication Attempts from the Suspicious IP

To better understand the attack timeline, all authentication attempts originating from the suspicious IP address were reviewed.

**Query used**

```kusto
SigninLogs
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where IPAddress == "205.147.16.190"
| project TimeGenerated, AppDisplayName, ResultType, IPAddress
| order by TimeGenerated asc
```

### Findings

The logs showed multiple authentication attempts from the same external IP before a successful login was achieved. This activity aligned with the repeated MFA notifications observed during the initial investigation.

<img width="1355" height="632" alt="image" src="https://github.com/user-attachments/assets/26c6b73b-c8ce-4dbc-8db2-938095a08671" />


---

## 4. Investigated Post-Authentication Activity

Once the attacker gained access, CloudAppEvents were reviewed to identify the first actions performed within the Microsoft 365 environment.

**Query used**

```kusto
CloudAppEvents
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where AccountDisplayName == "Mark Smith"
| project TimeGenerated, ObjectType, ObjectName, ActionType, Type
| order by TimeGenerated asc
```

### Findings

The first activity performed after authentication was the creation of new Outlook inbox rules.

This behavior is commonly associated with Business Email Compromise attacks because it allows attackers to maintain visibility into sensitive communications while reducing the likelihood of detection.

<img width="1082" height="382" alt="image" src="https://github.com/user-attachments/assets/73fdb678-f883-42c0-be5d-062eeac0cd9d" />


---

## 5. Investigated Outlook Inbox Rules

The inbox rule parameters were examined to determine their purpose and identify any persistence mechanisms.

**Query used**

```kusto
CloudAppEvents
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where ActionType == "New-InboxRule"
| project TimeGenerated, AccountDisplayName, RawEventData
```

### Findings

Two malicious inbox rules were identified.

### Rule One

The attacker configured an inbox rule that automatically forwarded emails containing financial keywords including:

- invoice
- payment
- wire
- transfer

Forwarded emails were sent to:

```
insights@duck.com
```

The rule was also configured with **StopProcessingRules = True**, preventing additional inbox rules from processing those messages.

### Rule Two

A second inbox rule was created with the name:

```
..
```

This rule automatically deleted messages containing keywords such as:

- suspicious
- phishing
- security
- compromised
- verify

The rule also used **StopProcessingRules = True** to ensure the messages were removed before the user could review them.

Together, these rules enabled the attacker to monitor financial conversations while hiding security notifications that might have alerted the victim to the compromise.

<img width="712" height="591" alt="image" src="https://github.com/user-attachments/assets/43c6abec-0136-4d19-96ba-680e25b1fa7a" />


---

## 6. Investigated Business Email Compromise Activity

EmailEvents were reviewed to determine whether fraudulent emails had already been sent from the compromised mailbox.

**Query used**

```kusto
EmailEvents
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where SenderFromAddress == "m.smith@lognpacific.org"
| where SenderIPv4 == "205.147.16.190"
| project TimeGenerated, Subject, RecipientEmailAddress, SenderFromAddress, SenderIPv4
| order by TimeGenerated asc
```

### Findings

A fraudulent internal email was identified.

**Sender**

```
m.smith@lognpacific.org
```

**Recipient**

```
j.reynolds@lognpacific.org
```

**Subject**

```
RE: Invoice #INV-2026-0892 - Updated Banking Details
```

**Sender IP**

```
205.147.16.190
```

The attacker attempted to redirect invoice payments by impersonating the compromised employee and sending fraudulent banking information to another member of the organization.

<img width="863" height="305" alt="image" src="https://github.com/user-attachments/assets/77a4688b-24a9-4994-9c4b-cda9f12426eb" />



---

## 7. Investigated Additional Cloud Resource Access

The investigation expanded beyond Outlook to determine whether the attacker accessed other Microsoft 365 services after compromising the account.

**Query used**

```kusto
SigninLogs
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where IPAddress == "205.147.16.190"
| project AppDisplayName, IPAddress
| distinct AppDisplayName, IPAddress
| order by AppDisplayName asc
```

### Findings

Following mailbox access, the attacker interacted with files stored within Microsoft OneDrive for Business.

Additional review of Azure AD Sign-in Logs identified successful authentication to:

- Microsoft OneDrive for Business
- SharePoint Online

The investigation confirmed the attacker expanded their activity beyond Outlook and accessed cloud storage resources associated with the compromised account.

<img width="883" height="555" alt="image" src="https://github.com/user-attachments/assets/ce4fac48-a1c8-4707-ad9d-ce020e3c29aa" />


---

## 8. Correlated the Attacker's Session

To verify that all observed malicious activity belonged to the same authenticated session, Azure AD Sign-in Logs were correlated with CloudAppEvents using the Azure AD Session ID.

The Session ID observed in the successful authentication matched the AADSessionId recorded within the Outlook inbox rule creation events.

**Session ID**

```
00225cfa-a0ff-fb46-a079-5d152fcdf72a
```

This correlation confirmed the following actions were performed during the same authenticated session:

- Successful Azure AD sign-in
- Inbox forwarding rule creation
- Inbox hiding rule creation
- Business Email Compromise activity
- OneDrive access
- SharePoint access

This provided high confidence that all malicious activity originated from the same threat actor session.


---

## 9. Reviewed Conditional Access Policies

The successful authentication event was reviewed to determine whether Microsoft Entra Conditional Access policies evaluated or blocked the attack.

**Query used**

```kusto
SigninLogs
| where TimeGenerated between (datetime(2026-02-25 21:00:00) .. datetime(2026-02-25 23:00:00))
| where IPAddress == "205.147.16.190"
| where ResultType == 0
| project TimeGenerated, AppDisplayName, ConditionalAccessStatus
```

### Findings

The successful authentication returned the following result:

```
ConditionalAccessStatus = notApplied
```

No Conditional Access policy was evaluated during the successful authentication. This allowed the attacker to authenticate successfully after the victim approved the MFA request.

<img width="797" height="512" alt="image" src="https://github.com/user-attachments/assets/83d14198-0978-41e0-8db2-407ca0d32955" />


---

# Chronological Event Timeline

### Step 1 – Initial MFA Fatigue Attack

The attacker repeatedly attempted to authenticate to the victim's Microsoft 365 account, generating multiple Microsoft Authenticator push notifications. Azure AD Sign-in Logs recorded repeated authentication attempts requiring strong authentication (Result Type 50074).

---

### Step 2 – Successful Authentication

After multiple push notifications, the victim approved one of the authentication requests.

The attacker successfully authenticated from:

```
205.147.16.190
```

This IP differed from the victim's normal sign-in location:

```
172.175.65.103
```

---

### Step 3 – Persistence Established

Immediately after authenticating, the attacker created two Outlook inbox rules.

The first rule automatically forwarded finance-related emails to an external mailbox.

The second rule automatically deleted emails containing security-related keywords.

These rules ensured the attacker could continue monitoring sensitive communications while reducing the likelihood of discovery.

---

### Step 4 – Business Email Compromise

After establishing persistence, the attacker sent a fraudulent invoice email from the compromised mailbox to another employee.

The email requested updated banking information, indicating an attempted Business Email Compromise (BEC).

---

### Step 5 – Cloud Resource Access

The attacker subsequently accessed Microsoft OneDrive for Business and SharePoint Online, demonstrating that the compromise extended beyond Outlook into additional Microsoft 365 services.

---

# MITRE ATT&CK Mapping

| Technique | Description |
|------------|-------------|
| T1621 | Multi-Factor Authentication Request Generation |
| T1564.008 | Hide Artifacts: Email Hiding Rules |

---

# Indicators of Compromise (IOCs)

| Indicator | Value |
|------------|-------|
| Compromised User | m.smith@lognpacific.org |
| Attacker IP | 205.147.16.190 |
| Legitimate User IP | 172.175.65.103 |
| External Forwarding Address | insights@duck.com |
| Fraud Recipient | j.reynolds@lognpacific.org |
| Session ID | 00225cfa-a0ff-fb46-a079-5d152fcdf72a |

---

# Threat Attribution

The tactics, techniques, and procedures observed during the investigation closely aligned with publicly documented activity associated with **Scattered Spider**.

Observed behaviors included:

- MFA fatigue attacks
- Business Email Compromise
- Outlook inbox rule persistence
- Cloud identity abuse
- Credential theft through infostealer logs

While attribution cannot be made with certainty based solely on these artifacts, the techniques observed are consistent with known Scattered Spider tradecraft.

---

# Implications

The attacker successfully compromised a Microsoft 365 account through an MFA fatigue attack, bypassing multi-factor authentication by convincing the victim to approve a malicious sign-in request.

Following authentication, the attacker established persistence using Outlook inbox rules that automatically forwarded financial communications to an external email account while deleting security-related messages. These actions enabled the attacker to remain hidden while monitoring sensitive business communications.

The attacker then attempted to conduct Business Email Compromise by sending fraudulent banking instructions to another employee and accessed additional cloud resources including Microsoft OneDrive for Business and SharePoint Online.

The investigation also determined that no Conditional Access policies were applied during the successful authentication, allowing the attack to proceed without additional verification or restrictions.

---

# Summary

This investigation confirmed a successful Business Email Compromise originating from an MFA fatigue attack against a Microsoft 365 user.

Using Microsoft Sentinel, Azure AD Sign-in Logs, CloudAppEvents, and EmailEvents, the complete attack lifecycle was reconstructed. The investigation identified the initial authentication attempts, confirmed the attacker's source IP address, documented malicious Outlook inbox rules, identified the fraudulent invoice email, verified access to OneDrive and SharePoint, and correlated all malicious activity using the Azure AD Session ID.

The investigation concluded that the attacker established persistence, attempted financial fraud, and expanded access into cloud storage services after compromising the victim's account.

---

# Response Taken

The following containment actions were recommended:

- Revoke all active user sessions.
- Reset the compromised user's password.
- Require MFA re-registration.
- Remove malicious Outlook inbox rules.
- Review mailbox forwarding settings.
- Review OneDrive and SharePoint activity for unauthorized access.
- Investigate additional persistence mechanisms.
- Review and strengthen Conditional Access policies to mitigate future MFA fatigue attacks.

---
