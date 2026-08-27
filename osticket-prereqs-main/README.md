<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- download the osTicket-Installation-Files.zip
- Install / Enable IIS in Windows WITH CGI
- install PHP Manager for IIS (PHPManagerForIIS_V1.5.0.msi)
- install the Rewrite Module (rewrite_amd64_en-US.msi)
- Create the directory C:\PHP and unzip (php-7.3.8-nts-Win32-VC15-x86.zip) into the folder
- install VC_redist.x86.exe.
- install MySQL 5.5.62 (mysql-5.5.62-win32.msi)


<h2>Installation Steps</h2>

<p>
<img src="https://i.imgur.com/SAPqiLy.png" height="80%" width="80%" alt=""/>
</p>
<p>
In this lab, I began by creating an Azure Windows 10 virtual machine named osticket-vm with 4 vCPUs, using the username labuser and the password osTicketPassword1!, then connected to it through Remote Desktop. 

<p>
<img src="https://i.imgur.com/f0dfIoA.png" height="80%" width="80%" alt=""/>
</p>
<p>
  
Once inside the VM, I downloaded and unzipped the osTicket-Installation-Files.zip to the desktop, which created a folder containing all of the required installation files. The next step was enabling IIS with CGI by going into Windows Features and checking the CGI option under World Wide Web Services. After IIS was installed, I installed PHP Manager for IIS and the Rewrite Module from the provided folder.
</p>
<br />


<p>
<img src="https://i.imgur.com/LP4YGVf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
The next step was enabling IIS with CGI by going into Windows Features and checking the CGI option under World Wide Web Services. After IIS was installed, I installed PHP Manager for IIS and the Rewrite Module from the provided folder.
</p>
<br />


<p>
<img src="https://i.imgur.com/A7UIdaj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
 After IIS was installed, I installed PHP Manager for IIS and the Rewrite Module from the provided folder.
</p>
<br />



<p>
<img src="https://imgur.com/a/bDDkc8q" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/Ra92RIx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Next, I created a directory at C:\PHP and unzipped PHP 7.3.8 into it, followed by installing the VC_redist.x86.exe runtime. I then installed MySQL 5.5.62 using the Typical setup option, ran the configuration wizard with a Standard configuration, and set the root username and password both to root. With that completed, I opened IIS as Administrator, registered PHP in IIS by pointing PHP Manager to C:\PHP\php-cgi.exe, and restarted IIS to apply the changes.
</p>
<br />


<p>
<img src="https://i.imgur.com/TFZfVu9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Afterward, I unzipped osTicket v1.15.8 from the installation folder, copied the “upload” folder into C:\inetpub\wwwroot, and renamed it to “osTicket.” Reloading IIS allowed me to browse to the osTicket site, where I confirmed some extensions were missing. To fix this, I returned to IIS, opened PHP Manager, and enabled php_imap.dll, php_intl.dll, and php_opcache.dll. Once enabled, refreshing the osTicket site showed that the extensions were now active.
</p>
<br />

<p>
<img src="https://i.imgur.com/oRwAiq5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then configured the osTicket files by renaming ost-sampleconfig.php to ost-config.php and changing its permissions to remove inheritance, giving Everyone full control. Back in the browser setup, I provided the helpdesk name and a default email. I installed HeidiSQL from the folder, connected to MySQL using root/root, and created a database called osTicket. Returning to the browser, I entered the database name along with the root credentials and clicked Install Now, completing the installation successfully.
</p>
<br />


<p>
<img src="https://i.imgur.com/LSA5UQM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, I secured the installation by deleting the setup folder from C:\inetpub\wwwroot\osTicket and setting the ost-config.php file to read-only. At this point, the osTicket installation was complete, with the admin panel accessible at http://localhost/osTicket/scp/login.php and the end-user portal at http://localhost/osTicket/.
</p>
<br />
