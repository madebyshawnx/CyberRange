<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Ticket Lifecycle: Intake Through Resolution</h1>
This tutorial outlines the lifecycle of a ticket from intake to resolution within the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>Ticket Lifecycle Stages</h2>

- Intake
- Assignment and Communication
- Working the Issue
- Resolution

<h2>Lifecycle Stages</h2>

<p>
<img src="https://i.imgur.com/76yPsW0.png"/>
</p>
<p>
I started by logging into the Admin/Analyst panel at http://localhost/osTicket/scp/login.php and noted that the end-user portal was accessible at http://localhost/osTicket. The goal of this lab was to practice ticket intake from end users, observe ticket properties, assign priorities, and complete tickets as help desk agents. Before beginning, I changed the SysAdmins department to a top-level department and deleted the Maintenance department to simplify the structure.
</p>
<br />

<p>
<img src="https://i.imgur.com/vvwcVIH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I created a ticket as an end user stating that the entire mobile/online banking system is down. As the agent John, I observed the ticket’s properties, including priority, department, SLA, and assigned staff. I then set the ticket properties to Sev-A (1 hour, 24/7) and assigned it to the Online Banking department. After reassignment, John could no longer manage the ticket, so the agent Jane completed it.
</p>
<br />

<p>
<img src="https://i.imgur.com/vrDbvSy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then created another ticket as an end user reporting that the accounting department needed an Adobe upgrade. John observed the ticket properties and set the SLA to Sev-B (4 hours, 24/7) with assignment to the Support department. John was able to work this ticket through to completion. A third ticket was created reporting that the CFO’s laptop would no longer turn on. John again reviewed the ticket’s properties and assigned it Sev-B (4 hours, 24/7) under the Support department, then worked the ticket to completion.
</p>
<br />



<p>
<img src="https://i.imgur.com/WJofbzz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After setting properties across all tickets, the SysAdmins Sev-A ticket became inaccessible to John. To continue, I switched to the Admin Panel and reassigned myself View-access to the SysAdmins department. Returning to the Agent Panel, I confirmed that while I could now see the escalated ticket, I could no longer make changes to it. This demonstrated how osTicket handles role-based access control and escalated tickets.
</p>
<br />




<p>
<img src="https://i.imgur.com/TMoXqEq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, I solved all tickets and reflected on how osTicket mirrors real-world help desk processes. In practice, tickets can arrive through many intake methods including phone calls, chat apps, emails, web forms, or in-person requests. While technicians are often asked to fix issues on the spot, it is important to create tickets for every task to ensure proper metrics and tracking. I also noted that osTicket includes an email notification system, so whenever an update is made, the end user receives an email and can reply directly. Repeating this lab multiple times helps build intuition and reinforces the technical skill pillar by strengthening troubleshooting and system administration ability.
</p>
<br />





