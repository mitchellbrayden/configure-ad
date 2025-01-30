<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Install active directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/PKHWK1s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Server Manager on the Windows Server virtual machine(DC-1).
Click Add Roles and Features.
In the pop-up wizard:
Click Next until you reach the Server Roles section.
In the Server Roles section:
Check the box for Active Directory Domain Services.
When prompted, click Add Features to include the required features.
Continue with the wizard to complete the role installation.
</p>
<br />

<p>
<img src="https://i.imgur.com/LQDxI8A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Server Manager on the Windows Server virtual machine.

Click on the flag icon in the top-right corner, then select Promote this server to a domain controller.

In the Active Directory Domain Services Configuration Wizard:

Select Add a new forest.
For the Root domain name, enter mydomain.com.
In the DNS Options section:

Uncheck Create DNS Delegation.
Complete the wizard by following the remaining steps.

Once the wizard finishes, restart the virtual machine.

After the restart, log in to the virtual machine using: mydomain.com\yourUsername.
</p>
<br />

<p>
<img src="https://i.imgur.com/LYgMAsJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After logging back in, click on the Start menu (Windows icon).

Open the Windows Administrative Tools folder.

Select Active Directory Users and Computers.

In the Active Directory Users and Computers window:

Expand mydomain.com in the left-hand menu.
To create the first organizational unit:

Right-click on mydomain.com, then select New > Organizational Unit.
Name the unit _EMPLOYEES and click OK.
To create the second organizational unit:

Right-click on mydomain.com again, select New > Organizational Unit.
Name the unit _ADMINS and click OK.
</p>
<br />

<p>
<img src="https://i.imgur.com/uxroTDB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Active Directory Users and Computers.

Expand mydomain.com, then expand the _ADMINS organizational unit.

Create the user:

Right-click on _ADMINS, then select New > User.
In the wizard:
Enter Jane as the first name, Doe as the last name, and jane_admin as the user logon name.
Click Next.
Set the password to Cyberlab123! and ensure the appropriate options (e.g., "User must change password at next logon") are unchecked.
Click Finish to create the user.
Assign the user to the Domain Admins group:

Right-click on Jane Doe in the _ADMINS folder, then select Add to a group.
In the text box at the bottom, type Domain Admins and click Check Names.
Ensure the group name is validated, then click OK.
Log out of the virtual machine.

Log back in using the credentials:

Username: mydomain.com\jane_admin
Password: Cyberlab123!
</p>
<br />

<p>
<img src="https://i.imgur.com/XRFBJGq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Retrieve DC-1's Private IP Address:

Open the Microsoft Azure Portal.
Navigate to the Virtual Machines section.
Select DC-1 from the list of virtual machines.
In DC-1's Overview, locate and copy its Private IP Address.
Configure Client-1's Network Settings:

Open Client-1 in the Azure portal.
Go to Networking and select the Network Interface associated with Client-1.
Under Settings, select IP Configurations.
Click on the IP configuration to open its details.
Set a Custom DNS Server:

On the left-hand menu, select DNS Servers.
Choose Custom as the DNS server type.
Paste DC-1's Private IP Address into the DNS Server field.
Click Save to apply the changes.
Restart Client-1:

Navigate back to the Virtual Machines section.
Select Client-1, then click Restart to apply the updated network settings.
After the restart, Client-1 will use DC-1 as its DNS server.
</p>
<br />

<p>
<img src="https://i.imgur.com/d2sIZsF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Join Client-1 to the Domain:
Access Client-1 via Remote Desktop:

Log in to Client-1 using Remote Desktop.
Open System Settings:

Right-click on the Start Menu (Windows icon).
Click on System.
Rename or Join a Domain:

On the right-hand side of the System window, click Rename this PC (advanced).
In the popup, click Change.
Join the Domain:

Under Member of, check the box labeled Domain.
Enter mydomain.com in the domain field.
Click OK.
Provide Domain Admin Credentials:

When prompted, enter the following credentials:
Username: mydomain\jane_admin
Password: Cyberlab123!
Click OK.
Restart Client-1:

A popup will confirm that the computer has successfully joined the domain. Click OK.
Restart Client-1 to finalize the changes.
After restarting, Client-1 will be part of the mydomain.com domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/a5vA1TX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Verify and Organize Client-1 in Active Directory:
Log In to DC-1:

Access the DC-1 virtual machine through Remote Desktop.
Open Active Directory Users and Computers:

Click on Start (Windows icon).
Navigate to Windows Administrative Tools and select Active Directory Users and Computers.
Verify Client-1 in the Computers Folder:

Expand mydomain.com in the left-hand pane.
Click on the Computers folder.
Confirm that Client-1 appears in the list.
Create a New Organizational Unit:

Right-click on mydomain.com.
Select New > Organizational Unit.
Name the new unit _CLIENTS.
Click OK.
Move Client-1 to the _CLIENTS Unit:

Drag and drop Client-1 from the Computers folder to the _CLIENTS folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/wzjyNBQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Steps to Set Up Remote Desktop for Non-Administrative Users on Client-1:
Log In to Client-1 as a Domain Admin:

Use mydomain.com\jane_admin as the username and Cyberlab123! as the password.
Access System Settings:

Right-click on the Start (Windows icon).
Select System.
Enable Remote Desktop:

On the right-hand side, click on Remote Desktop.
Ensure Remote Desktop is toggled ON.
Select Users for Remote Access:

Under the Remote Desktop section, click Select users that can remotely access this PC.
In the new window, click Add.
Add Domain Users:

In the text box, type Domain Users.
Click Check Names to verify the entry.
Once resolved, click OK.

Close the Remote Desktop settings windows.
Test Remote Login:

Log out of Client-1 and attempt to log in remotely as a non-administrative domain user to verify access.
Outcome:
Non-administrative domain users can now remotely access Client-1 using Remote Desktop.
</p>
<br />
