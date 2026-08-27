# Automating-User-Role-Management-in-Microsoft-Entra-ID-with-PowerShell

In this lab, I automated identity lifecycle tasks inside my Microsoft Entra ID (Azure AD) tenant using PowerShell 7 and the Microsoft Graph module.
The automation created users from a CSV file, organized them into security groups, assigned departments, and delegated administrative roles simulating how IAM engineers use scripting to manage users at scale instead of relying only on the Azure portal.

This lab builds on the secure tenant created previously and focuses on PowerShell-based automation for provisioning and access management.

## Objectives

By the end of this lab, I completed the following:

-Imported user data from a .csv file into Entra ID using PowerShell

-Automatically created 10 cloud-only users with custom passwords

-Created and organized department-based security groups (HR, IT, Finance)

-Added users to their respective groups using conditional logic

-Updated user department attributes programmatically

-Created a static and dynamic security group for Finance users

-Assigned the User Administrator role to the Helpdesk Admin account using Microsoft Graph

---

<img width="1400" height="808" alt="image" src="https://github.com/user-attachments/assets/b8bfc939-bbfb-4c12-9c53-c97d23b31919" />


# Tools Used

-PowerShell 7

-Microsoft Graph PowerShell module

-Azure Portal (Microsoft Entra ID)

-CSV user data file

-Microsoft Graph API endpoints (via PowerShell)

---

### Importing Users from CSV

To start, I wrote a PowerShell script (Create-Users.ps1) that reads user information from a .csv file containing DisplayName, UserPrincipalName, and Password columns.
The script loops through each record and creates Entra ID users automatically using New-MgUser.

(This step automated user provisioning instead of manually creating each account in the portal.)

<img width="682" height="421" alt="ss2 creating user accounts_pw csv" src="https://github.com/user-attachments/assets/0118707f-5054-43c6-afe9-247e46136a2c" />

<img width="1048" height="803" alt="ss3 user creation success" src="https://github.com/user-attachments/assets/fb783b6f-4d67-432c-9007-c719608c9b4b" />


---

### Creating and Assigning Security Groups

Next, I created three department-based groups — HR_Team, IT_Team, and Finance_Team — using PowerShell (Create-Groups.ps1).
Each group was verified in Entra ID and populated with users automatically based on their display name patterns (User01–User03 for HR, etc.).
(This simulates how IAM engineers use automation to manage large organizational structures dynamically.)

<img width="1057" height="462" alt="ss4 successful creation of groups" src="https://github.com/user-attachments/assets/04a090d8-1c0d-4bf5-b9d5-e4ee8163198c" />


---

### Assigning Departments

After group creation, I ran the Set-Departments.ps1 script to update each user’s Department attribute in Entra ID according to their group membership.
The script used Update-MgUser to set HR, IT, or Finance for each user. (This aligns identity metadata across the directory)

<img width="1123" height="610" alt="ss5 Departments set" src="https://github.com/user-attachments/assets/d374abe2-056e-47b4-b8bd-337fad5a2419" />


---

###  Creating Dynamic and Static Finance Groups

To explore advanced automation, I created both static and dynamic Finance department groups.

The static group was manually populated via PowerShell.

The dynamic group was configured (attempted) to include all users where Department equals “Finance.”

<img width="1161" height="300" alt="ss6 list of finance static members dept" src="https://github.com/user-attachments/assets/4fac1f88-3fac-4e87-9e18-45b502973ff1" />


---

### Assigning Administrative Roles

Finally, I automated role delegation using Microsoft Graph PowerShell.
I retrieved the User Administrator role ID and added the Helpdesk Admin account as a member.
This simulated granting delegated administrative privileges securely and reproducibly.

<img width="961" height="257" alt="ss7 created helpdesk admin user " src="https://github.com/user-attachments/assets/c3a3f597-55f2-4004-ab12-57797034e06e" />


(below is a snapshot verifying the admin was created successfuly through powershell by checking the GUI web portal)

<img width="1918" height="507" alt="ss8 web portal confirmation" src="https://github.com/user-attachments/assets/c5cbe1cd-f48a-4020-8697-9faf5b21eeb9" />




---

### Reflection 

This lab demonstrated how identity engineers use PowerShell automation to manage users, groups, and roles efficiently.
Instead of manually creating accounts and assignments in the GUI, I leveraged Microsoft Graph PowerShell to handle the full IAM lifecycle — provisioning, grouping, department tagging, and delegated administration.
By using scripts like Create-Users.ps1, Create-Groups.ps1, and Set-Departments.ps1, I built a repeatable and scalable IAM process, reducing manual error and preparing the tenant for enterprise-level automation workflows.

