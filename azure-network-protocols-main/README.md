<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Resource Group in Azure and deploy two VMs (Windows 10 and Ubuntu) on the same virtual network/subnet.
- Connect to the Windows VM with Remote Desktop, install Wireshark, and capture ICMP traffic while pinging the Ubuntu VM and public websites.
- Use Wireshark to observe firewall blocking (ICMP), SSH sessions, DHCP renewals, DNS lookups, and RDP traffic.
- Close Remote Desktop and clean up by deleting the Resource Group to remove all resources.

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/UqM2QAy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I began by logging into the Azure Portal and creating a new Resource Group. Within that group, I deployed a Windows 10 VM, allowing Azure to automatically create a new virtual network and subnet. I then created a Linux (Ubuntu) VM in the same Resource Group, making sure it used the exact same virtual network and subnet. Both machines used simple username/password authentication. At the end of this section, both VMs were running and ready for Part 2.
</p>
<br />

<p>
<img src="https://i.imgur.com/AtbQ62j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I connected to the Windows 10 VM through Remote Desktop. Inside the VM, I installed Wireshark and began a packet capture. By filtering only ICMP traffic, I could clearly see the results when I pinged the Ubuntu VM’s private IP address from the Windows VM—requests and replies showed up in Wireshark. I also tested by pinging a public website like www.google.com and confirmed the ICMP traffic displayed in real-time.
</p>
<br />

<p>
<img src="https://i.imgur.com/ZwTMRJs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/vxzSypL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
I started a continuous ping from the Windows VM to the Ubuntu VM and then opened the Ubuntu VM’s Network Security Group (NSG) in Azure. Disabling inbound ICMP immediately caused the pings to fail and Wireshark showed no replies. Re-enabling ICMP allowed the pings to succeed again. After this, I moved on to SSH. With Wireshark filtering for SSH traffic, I used PowerShell on the Windows VM to connect to the Ubuntu VM (ssh labuser@<private IP>). Every command I typed created visible SSH packets, and when I typed exit, the SSH session closed.
</p>
<br />

<p>
<img src="https://i.imgur.com/NRpPLbE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then observed DHCP traffic by filtering for DHCP in Wireshark and running ipconfig /renew in PowerShell as admin on the Windows VM. This generated clear DHCP request and response packets. For DNS traffic, I filtered for DNS in Wireshark and used nslookup to resolve google.com and disney.com, watching the query and reply traffic appear in real time. Finally, I filtered for RDP traffic (tcp.port == 3389). Wireshark showed nonstop traffic, which made sense since RDP constantly streams the live desktop from one computer to another instead of only sending traffic when specific actions are performed.
</p>
<br />

<p>
</p>
<p>
To finish, I closed the Remote Desktop session and returned to the Azure portal. I deleted the Resource Group that contained both virtual machines, confirming that all associated resources were removed. This ensured the lab environment was cleaned up and costs were no longer running.
</p>
<br />
