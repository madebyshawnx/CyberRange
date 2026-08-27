<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

-Deploy and configure a new domain controller (DC-1) with Active Directory Domain Services and create the domain (mydomain.com). 
- Create Organizational Units (OUs) for employees, admins, and clients. Add a domain admin account (jane_admin) and promote it for ongoing management.  
- Join Client-1 to the new domain, verify membership in ADUC, and organize it under the _CLIENTS OU.  
- Configure remote desktop access for domain users on Client-1, bulk-create employee accounts with a PowerShell script, and validate login with new user credentials. 


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/EQ2c2TH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I began the lab by turning on the DC-1 and Client-1 virtual machines in the Azure portal. Once DC-1 was running, I logged in and installed Active Directory Domain Services, then promoted the machine to a domain controller by creating a new forest named mydomain.com. After the restart, I logged back into DC-1 using the domain account mydomain.com\labuser.
</p>
<br />

<p>
<img src="https://i.imgur.com/f6Z268v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I created domain admin accounts. Inside Active Directory Users and Computers (ADUC), I created an Organizational Unit (OU) named _EMPLOYEES and another OU named _ADMINS. Within the _ADMINS OU, I created a new user account called Jane Doe with the username jane_admin and password Cyberlab123!. I added jane_admin to the Domain Admins security group, logged out of DC-1, and then logged back in as mydomain.com\jane_admin, which became my permanent administrator account.
</p>
<br />

<p>
<img src="https://i.imgur.com/Hv776zR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then joined Client-1 to the domain. Logging into Client-1 as the original local admin account (labuser), I changed the computer settings to join the mydomain.com domain, which forced a restart. Returning to DC-1, I verified in ADUC that Client-1 appeared as a domain member. To keep things organized, I created an OU named _CLIENTS and moved Client-1 into it. At this stage, the core domain setup was complete. If stopping for the day, I powered down the VMs from the Azure portal instead of deleting them, since they would be needed for future labs.
</p>
<br />



<p>
<img src="https://i.imgur.com/lPCLBv9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For Part 2, I again turned on both DC-1 and Client-1 from the Azure portal. On Client-1, I logged in as mydomain.com\jane_admin and enabled Remote Desktop through system properties. I allowed domain users remote access, which ensured that regular, non-administrative accounts could now log into Client-1. In a real-world environment, this would typically be done with Group Policy so the setting could be applied across many systems at once.
</p>
<br />



<p>
<img src="https://i.imgur.com/CUqTBlB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/Asx2F1F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

The final step was to bulk-create domain user accounts. On DC-1, I logged in as jane_admin, opened PowerShell ISE as administrator, and pasted in a provided script. Running the script automatically created multiple employee accounts inside the _EMPLOYEES OU. After verifying in ADUC that the accounts had been added, I tested the setup by logging into Client-1 with one of the new accounts using the password defined in the script. This confirmed that domain users could access the machine via Remote Desktop.
</p>
<br />





To finish the lab, I left the environment intact since future exercises will build on it. To save costs in Azure, I simply stopped the VMs from the portal instead of deleting them.
</p>
<br />

