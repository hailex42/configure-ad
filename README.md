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

-  Preparing Active Directory Infrastructure
-  Deploying Active Directory
-  Creating Users with Power Shell
-  Group Policy and Managing Accounts


![image](https://github.com/user-attachments/assets/ed823a33-e651-497e-822e-3b88b0594c04)

<h3>Preparing Active Directory Infrastructure</h3>
<p>

To set up a Domain Controller in Azure, create a Resource Group, then set up a Virtual Network and Subnet. 

Deploy a Windows Server 2022 VM (DC-1) with a static Private IP, disable the Windows Firewall, and configure it as a Domain Controller. 

Next, create a Windows 10 Client VM (Client-1), attach it to the same Virtual Network, and configure its DNS settings to point to DC-1’s Private IP. Restart Client-1, verify connectivity by pinging DC-1, and confirm DNS settings with ipconfig /all in PowerShell. 

These steps establish the foundation for further Active Directory configurations in Azure.


</p>
<br />

![image](https://github.com/user-attachments/assets/9a8a322b-5d82-4e03-a313-25b8152bbe45)

<h3>Deploying Active Directory</h3>
<p>
Install Active Directory
  
Log into DC-1 and install Active Directory Domain Services.

Promote DC-1 to a Domain Controller, creating a new forest (e.g., mydomain.com).

Restart and log back in as mydomain.com\labuser.

Create a Domain Admin User

In Active Directory Users and Computers (ADUC), create the following OUs:
_EMPLOYEES

_ADMINS

Create a user Jane Doe (jane_admin / Cyberlab123!).

Add jane_admin to the Domain Admins security group.

Log out and back in as mydomain.com\jane_admin—this is now your admin account.

Join Client-1 to the Domain

Ensure Client-1’s DNS is set to DC-1’s Private IP.

Restart Client-1 from the Azure Portal.

Log into Client-1 as labuser and join it to the domain.

Verify Client-1 appears in ADUC, then move it to the _CLIENTS OU.

![image](https://github.com/user-attachments/assets/7b5c660e-da04-4b2a-a31b-a85781a5c911)

<h3>Creating Users with Power Shell</h3>
<p>
Enable Remote Desktop for Domain Users.
  
Log into Client-1 as jane_admin.

Open System Properties → Remote Desktop → Allow domain users.

You can now log into Client-1 as a standard domain user.

Create Additional Users & Test Login.

Log into DC-1 as jane_admin.

Open PowerShell_ISE as Administrator and run a script to create users.

Verify new users in ADUC under _EMPLOYEES.

Attempt to log into Client-1 with one of the newly created accounts

</p>
<br />

![image](https://github.com/user-attachments/assets/03a08c15-81f8-47f6-afb2-f146839dc457)
<h3>Group Policy and Managing Accounts</h3>

<p>
Simulating an Account Lockout

Log into DC-1.

Select a previously created user account.

Attempt to log in 10 times with an incorrect password.

Configuring Group Policy for Lockout Threshold

Set the Account Lockout Threshold in Group Policy to 5 failed attempts.

Attempt to log in 6 times with an incorrect password.

Verify that the account is locked out in Active Directory.

Unlock the account, reset the password, and attempt to log in successfully.

Enabling and Disabling User Accounts

Disable the account in Active Directory.

Attempt to log in and observe the error message.

Re-enable the account and verify successful login.

Reviewing Logs

Observe event logs on the Domain Controller.

Check related logs on the Client Machine.
</p>
<br />
