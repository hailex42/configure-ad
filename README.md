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

<h3>Preparing Active Directory Infrastructure</h3>

- ### [Youtube: Active Directory Infrastructure](https://youtu.be/NdzmEvoZbUU)

<p>

To set up a Domain Controller in Azure, create a Resource Group, then set up a Virtual Network and Subnet. 

Deploy a Windows Server 2022 VM (DC-1) with a static Private IP, disable the Windows Firewall. 

Next, create a Windows 10 Client VM (Client-1), attach it to the same Virtual Network, and configure its DNS settings to point to DC-1’s Private IP. Restart Client-1, verify connectivity by pinging DC-1, and confirm DNS settings with ipconfig /all in PowerShell. 

These steps establish the foundation for further Active Directory configurations in Azure.


</p>
</br>

<h3>Deploying Active Directory</h3>

- ### [Youtube: Deploying Active Directory](https://youtu.be/K609TSbxmj0)


<h4>Install Active Directory</h4>

<p> Server Manager>Add Roles and Features>Role-based or feature-based installation>Select a server from the server pool>select Active Directory Domain Services>Install

- In Server Manager Dashboard

Click the flag and select 'Promote this server to a domain controller'>Deployment Configuration>Add a new forest>Root domain name: (e.g., nero.com).>Install </p>

<p>
- Restart and log back in as nero.com\labuser.

In Active Directory Users and Computers (ADUC)>Right cick your domain name> create the following Organizaitonal Units (OU):
_EMPLOYEES

_ADMINS
</p> 

![image](https://github.com/user-attachments/assets/9a8a322b-5d82-4e03-a313-25b8152bbe45)

<p>

Create a user in the _Admins OU 
  
Name user Jane Doe, User logon name:jane_admin@nero.com Password:(eg.Forester23!)

Add jane_admin to the Domain Admins security group. (Right click Jane Doe user in the admins OU, navigate to 'Member Of' tab, click add, 
enter 'domain admins' in the 'enter the object name to select' field, then click ok.

Log out and back in as mydomain.com\jane_admin (this is now your admin account).
</p>

<h4>Now to join Client-1 to the domain controller:</h4>

<p>
Log into Client-1.

      -  Go to systems settings, click rename this PC, in the Computer Name tab click Change to rename computer.
      -  In the 'Member of'section; select Domain and input the name of the domain: (eg. nero.com).
      -  Input logon credentials for nero.com\jane_admin (This account has permission to join the domain).
      -  Go back to the DC->ADUC->Nero.com
      -  Create new _Clients OU
      -  Go back to the DC->ADUC->Nero.com
      -  Select Computers folder. 
      -  Verify that client-1 appears in there.
      -  Drag the client-1 into the _Clients OU.
      
(Ensure Client-1’s DNS is set to DC-1’s Private IP).

(Restart Client-1 from the Azure Portal).

</p>
</br>

<h3>Creating Users with Power Shell</h3>

- ### [Youtube: Creating Users with Power Shell](https://youtu.be/ETuLQhwHp9s)


<p>
  Log into Client-1 as mydomain.com\jane_admin.
  
      - Open system properties.
      - Click “Remote Desktop”
      - Then Click "select users that can remotely access this PC"
      - Remote Desktop Users> Add
      - Select Users or Groups> In the 'Enter the object names to select' fied, type: 'Domain users' and then click check names... then click ok.

You can now log into Client-1 as a normal, non-administrative user now
</p>

<p>
Log into DC-1 as jane_admin.

Open PowerShell_ISE as Administrator> Click create new script> Paste and save script> run script (to create users).

Verify new users in ADUC under _EMPLOYEES.

Attempt to log into Client-1 with one of the newly created accounts

</p>
<br />

![image](https://github.com/user-attachments/assets/7b5c660e-da04-4b2a-a31b-a85781a5c911)


<h3>Group Policy and Managing Accounts</h3>

- ### [Youtube: Account Lockouts](https://www.youtube.com/watch?v=Exgmu6gjWGI)

  ![image](https://github.com/user-attachments/assets/77a8978f-abaa-4764-adf7-5f5a1099a65a)


<p>
  Login Attempt
  
      -Log into DC-1 as jane_admin
      -Select a previously created user account.
      -Attempt to log into client-1 with the user account
      -Try to login 10 times with an incorrect password.

Configuring Group Policy for Lockout Threshold

      - In the Domain controller, right click start and select run
      - Type gpmc.msc> enter
      - This opens the Group Policy Management Console
      - Go to domains>(your domain)>right click default domain policy> edit
      - In default domain policy> Computer Configuration> Policies> Windows Settings> Security Settings> Account Policy> Account lockout Policy
      -Set the Account Lockout Threshold in Group Policy to 5 failed attempts.
      -Attempt to log in 6 times with an incorrect password.

Verify that the account is locked out in Active Directory.

Unlock the account, reset the password, and attempt to log in successfully. </p> </br>



- ### [Youtube: Enabling & Disabling Accounts](https://www.youtube.com/watch?v=rrAw88eMW74)

<p> Disable the account in Active Directory.
Attempt to log in and observe the error message.
Re-enable the account and verify successful login.
</p>


- ### [Youtube: Reviewing Logs](https://www.youtube.com/watch?v=Onkr8PGxfp8)

<p> Observe event logs on the Domain Controller.
Check related logs on the Client Machine.</p></br>


