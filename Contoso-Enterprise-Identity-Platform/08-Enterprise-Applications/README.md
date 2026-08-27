# Enterprise Applications (SSO, SAML, OAuth 2.0 & OpenID Connect)

## Project Overview

Designed and implemented enterprise application authentication using Microsoft Entra ID by configuring Single Sign-On (SSO), SAML federation, OAuth 2.0/OpenID Connect application registration, Microsoft Graph API permissions, and client authentication. This project demonstrates how organizations securely integrate third-party SaaS applications and internally developed applications into a centralized identity platform while enforcing modern authentication and authorization.

---

# Objectives

- Configure Enterprise Applications in Microsoft Entra ID
- Implement SAML-based Single Sign-On (SSO)
- Register a custom application using App Registrations
- Configure OAuth 2.0 / OpenID Connect authentication
- Configure Redirect URIs
- Generate and manage Client Secrets
- Configure Microsoft Graph API permissions
- Understand Delegated vs. Application permissions
- Demonstrate modern enterprise authentication architecture

---

# Environment

| Component | Technology |
|-----------|------------|
| Identity Provider | Microsoft Entra ID |
| Enterprise SaaS Application | Dropbox Business |
| Custom Application | ACHATECH HR Portal |
| Authentication Protocols | SAML 2.0, OpenID Connect |
| Authorization Framework | OAuth 2.0 |
| API Platform | Microsoft Graph |
| Client Authentication | Client Secret |
| Access Management | Microsoft Entra Enterprise Applications |

---

# Architecture

> **Figure 1. Enterprise Application Authentication Architecture**

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/70366489-1e25-4239-88db-14fbd63a0edb" />


This architecture demonstrates Microsoft Entra ID acting as the centralized Identity Provider (IdP) for both third-party SaaS applications and internally developed applications. Enterprise applications authenticate users through SAML-based Single Sign-On, while custom applications authenticate using OAuth 2.0/OpenID Connect before securely accessing Microsoft Graph APIs.

---

# Project Walkthrough

## 1. Enterprise Application Integration

Added **Dropbox Business** as an Enterprise Application from the Microsoft Entra Gallery to simulate onboarding a third-party SaaS application into the organization's identity platform.

Configured:

- Enterprise Application
- Application assignment
- User access management

> **Figure 2. Dropbox Business Enterprise Application deployed within Microsoft Entra ID to simulate third-party SaaS integration into the organization's centralized identity platform.**

<img width="951" height="510" alt="image" src="https://github.com/user-attachments/assets/e25672a2-57e9-4b79-b0c8-a8439a303e8b" />


---

## 2. Role-Based Application Access

Assigned the **ACHATECH Finance** security group to the Dropbox Business Enterprise Application.

Rather than assigning individual users, access is managed through security groups, allowing new employees to automatically inherit application access when added to the appropriate department group.

### Concepts Demonstrated

- Role-Based Access Control (RBAC)
- Group-Based Application Assignment
- Least Privilege Access

> **Figure 3. The Dropbox Business Enterprise Application was assigned to the ACHATECH-Finance security group, demonstrating group-based access management and Role-Based Access Control (RBAC). Users inherit application access through security group membership rather than individual assignments, simplifying administration and supporting the principle of least privilege.**

<img width="844" height="423" alt="image" src="https://github.com/user-attachments/assets/eceae00f-f591-49a7-9f5a-56934f1a4004" />


---

## 3. SAML Single Sign-On (SSO)

Configured SAML-based Single Sign-On within Microsoft Entra ID.

Reviewed the trust relationship established between:

- Microsoft Entra ID (Identity Provider)
- Dropbox Business (Service Provider)

Covered:

- Entity ID
- Reply URL (Assertion Consumer Service URL)
- Signing Certificate
- Federation Metadata
- Attributes & Claims

This configuration prepares the application for federated authentication while centralizing identity management within Microsoft Entra ID.

> **Figure 4. SAML-based Single Sign-On (SSO) configuration for Dropbox Business. Microsoft Entra ID is configured as the Identity Provider (IdP), establishing a trust relationship with the Service Provider through SAML metadata, certificates, attributes and claims, and federation settings to enable centralized authentication.**

<img width="916" height="845" alt="image" src="https://github.com/user-attachments/assets/ffbfaae3-06da-461b-bade-74bb2c15976f" />

---

## 4. OAuth 2.0 / OpenID Connect Application Registration

Created a custom application (**ACHATECH HR Portal**) using Microsoft Entra App Registrations.

Configured:

- Single-Tenant Application
- Web Platform
- Redirect URI
- Authorization Code Flow

This registration allows internally developed applications to authenticate users using Microsoft Entra ID.

> **Figure 5. App Registration for the ACHATECH HR Portal within Microsoft Entra ID. The application was registered as a single-tenant web application and assigned a unique Client ID and Tenant ID, establishing its identity for OAuth 2.0 and OpenID Connect authentication.**

<img width="1324" height="397" alt="image" src="https://github.com/user-attachments/assets/b5656532-e5bb-4f5a-be8f-4f7ff57cbb16" />


---

## 5. Authentication Configuration

Configured the application's Web Platform authentication settings.

Configured:

- Redirect URI
- Authorization Code Flow
- Web Platform

The Redirect URI specifies where Microsoft Entra ID returns users after successful authentication.

```text
https://localhost/signin-oidc
```

This lab uses a localhost Redirect URI to simulate a development environment. In production, this would be replaced with the organization's public application URL.

> ** Figure 6. Authentication settings for the ACHATECH HR Portal configured as a web application. A Redirect URI (https://localhost/signin-oidc) was configured to receive authentication responses from Microsoft Entra ID after successful OAuth 2.0/OpenID Connect authentication, simulating a development environment.**

<img width="1132" height="611" alt="image" src="https://github.com/user-attachments/assets/9edefdd0-0469-4928-ad32-bc2e26f2e27d" />

---

## 6. Client Authentication

Generated a Client Secret for the ACHATECH HR Portal.

Client Secrets allow confidential applications to authenticate themselves to Microsoft Entra ID before tokens are issued.

This demonstrates application authentication in addition to user authentication.

### Security Considerations

- Client Secrets should never be stored in source code.
- Secrets should be rotated periodically.
- Production environments should store secrets in Azure Key Vault or another secure secrets manager.

> **Figure 7. Client Secret configuration for the ACHATECH HR Portal. The application uses a Client Secret to authenticate itself to Microsoft Entra ID before OAuth 2.0 tokens are issued. Secret values are treated as sensitive credentials and are securely stored outside of application source code**

<img width="891" height="518" alt="image" src="https://github.com/user-attachments/assets/1bd7ddcc-2ce1-42cd-9f1c-5deef8ec9bff" />


---

## 7. Microsoft Graph API Permissions

Configured Microsoft Graph delegated permissions for the application.

Covered:

- Microsoft Graph
- Delegated Permissions
- Application Permissions
- Admin Consent
- Least Privilege

This demonstrates how OAuth 2.0 controls what resources an authenticated application may access after successful authentication.

> **Figure 8. Microsoft Graph delegated API permissions configured for the ACHATECH HR Portal. The application was granted delegated permissions to access user profile information on behalf of authenticated users, demonstrating OAuth 2.0 authorization and the principle of least privilege.**

<img width="1055" height="592" alt="image" src="https://github.com/user-attachments/assets/dfca5254-2298-4e87-ace8-508e360441bc" />


---

# Authentication Flow

> **Figure 9. OAuth 2.0/OpenID Connect authentication flow illustrating how a user authenticates with Microsoft Entra ID, receives authentication tokens, and securely accesses Microsoft Graph resources through delegated permissions.**

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/36396fb7-627e-4356-a715-96dd14a3d6f4" />


Authentication Process:

1. User accesses the ACHATECH HR Portal.
2. The application redirects the user to Microsoft Entra ID.
3. The user authenticates using corporate credentials and MFA.
4. Microsoft Entra ID validates the application using its Client ID and Client Secret.
5. Microsoft Entra ID issues an Authorization Code.
6. The application exchanges the Authorization Code for an ID Token and Access Token.
7. The application uses the Access Token to securely access Microsoft Graph according to its assigned API permissions.
8. Microsoft Graph authorizes the request and returns the requested resources.

---

# Key Concepts Learned

- Enterprise Applications
- App Registrations
- Single Sign-On (SSO)
- Security Assertion Markup Language (SAML)
- OAuth 2.0
- OpenID Connect (OIDC)
- Authorization Code Flow
- Microsoft Graph
- Redirect URI
- Client ID
- Client Secret
- Delegated Permissions
- Application Permissions
- Admin Consent
- Identity Provider (IdP)
- Service Provider (SP)

---

# Skills Demonstrated

- Microsoft Entra ID Administration
- Identity & Access Management (IAM)
- Enterprise Application Integration
- SAML Federation
- OAuth 2.0 / OpenID Connect Configuration
- Microsoft Graph Authorization
- Secure Client Authentication
- Role-Based Access Control (RBAC)
- Enterprise Identity Architecture
- Modern Authentication

---

# Conclusion

This project demonstrates the implementation of modern enterprise authentication using Microsoft Entra ID. Third-party SaaS applications were integrated using Enterprise Applications and SAML-based Single Sign-On, while a custom internal application was registered using OAuth 2.0 and OpenID Connect. The project also implemented secure client authentication, Microsoft Graph API authorization, and role-based application access, illustrating how organizations centralize identity, authentication, and authorization across enterprise applications.

---

# Screenshot Checklist

| Figure | Screenshot |
|---------|------------|
| Figure 1 | Enterprise Application Architecture Diagram |
| Figure 2 | Enterprise Application Overview |
| Figure 3 | Users & Groups Assignment (Finance Group) |
| Figure 4 | SAML Configuration |
| Figure 5 | App Registration Overview |
| Figure 6 | Authentication (Redirect URI) |
| Figure 7 | Certificates & Secrets (**Secret Value Hidden**) |
| Figure 8 | API Permissions |
| Figure 9 | OAuth 2.0 / OpenID Connect Authentication Flow |
