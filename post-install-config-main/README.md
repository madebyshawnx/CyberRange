<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Post-Install Configuration</h1>
This tutorial outlines the post-install configuration of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>Post-Install Configuration Objectives</h2>

- Configure Roles for grouping permissions 
- Set up Departments to manage ticket visibility 
- Create Teams by combining agents across departments
- Configure Agents and Users
- Define SLA Plans and Help Topics to manage ticket workflows.

<h2>Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/qpDt7XH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this part of the lab, I logged into the Admin/Analyst page at http://localhost/osTicket/scp/login.php, while end users could access the customer portal at http://localhost/osTicket. I noted the difference between the Agent Panel (where staff manage tickets) and the Admin Panel (where administrators configure the system).
</p>
<br />

<p>
<img src="https://i.imgur.com/EVe3opl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I configured roles to group permissions by going to Admin Panel → Agents → Roles, and created a role called Supreme Admin with full permissions. After this, I set up departments (which control ticket visibility) under Admin Panel → Agents → Departments, creating a department called SysAdmins.
</p>
<br />

<p>
<img src="https://i.imgur.com/JctkFkj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then configured teams, which allow agents from different departments to be grouped together. Under Admin Panel → Agents → Teams, I created a team called Online Banking.
</p>
<br />




<p>
<img src="https://i.imgur.com/HGSG5Zn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For ticket creation settings, I went to Admin Panel → Settings → User Settings and unchecked the option that allows unregistered users to create tickets. This enforces registration and login before ticket submission.
</p>
<br />





<p>
<img src="https://i.imgur.com/efBoWh1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I added agents (workers) under Admin Panel → Agents → Add New. I created two agents: Jane (assigned to the SysAdmins department) and John (assigned to the Support department). Similarly, I added users (customers) under Agent Panel → Users → Add New, creating two users: Karen and Ken.
</p>
<br />




<p>
<img src="https://i.imgur.com/zKQLM99.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After setting up users, I configured SLAs under Admin Panel → Manage → SLA. I created three SLA plans: Sev-A (1 hour, 24/7), Sev-B (4 hours, 24/7), and Sev-C (8 hours, business hours only). These determine response time requirements for tickets.
</p>
<br />



<p>
<img src="https://i.imgur.com/61tf5L4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, I created help topics under Admin Panel → Manage → Help Topics, adding categories such as Business Critical Outage, Personal Computer Issues, Equipment Request, Password Reset, and Other. These topics help route tickets properly when customers create them.
</p>
<br />
