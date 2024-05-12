# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

1. Setup Resources in Azure
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
- Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

2. Ensure Connectivity between the client and Domain Controller
- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
- Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
- Check back at Client-1 to see the ping succeed

3. Install Active Directory
- Login to DC-1 and install Active Directory Domain Services
- Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
- Restart and then log back into DC-1 as user: mydomain.com\labuser

4. Create an Admin and Normal User Account in AD
- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
- Create a new OU named “_ADMINS”
- Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
- Add jane_admin to the “Domain Admins” Security Group
- Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
- User jane_admin as your admin account from now on


5. Join Client-1 to your domain (mydomain.com)
- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
- Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)


6. Setup Remote Desktop for non-administrative users on Client-1
- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user now
- Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

7. Create a bunch of additional users and attempt to log into client-1 with one of the users
- Login to DC-1 as jane_admin
- Open PowerShell_ise as an administrator
- Create a new File and paste the contents of the script into it
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

Finish.
- 

<h2>Deployment and Configuration Steps</h2>
<p>
<img src="https://i.imgur.com/MfKwcpK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To set up resources in Azure, start by creating the Domain Controller VM (Windows Server 2022) named “DC-1” within a specified Resource Group and Virtual Network (Vnet).
</p>
<br />

<p>
<img src="https://i.imgur.com/Y1xnkDw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Make sure to set the Domain Controller’s NIC Private IP address to be static.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, create the Client VM (Windows 10) named “Client-1” in the same Resource Group and Vnet. Verify that both VMs are in the same Vnet using Network Watcher or similar tools.
</p>
<br />


<p>
<img src="https://i.imgur.com/y0Vvalh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the VMs are set up, ensure connectivity between the Client and Domain Controller. Log in to Client-1 using Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> to check connectivity.
</p>
<br />


<p>
<img src="https://i.imgur.com/eShF2RF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <img src="https://i.imgur.com/YJtThwd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 On DC-1, enable ICMPv4 in the local Windows Firewall to allow ping responses. Verify the successful ping from Client-1 to DC-1 to confirm connectivity.
</p>
<br />


<p>
<img src="https://i.imgur.com/LhUJiQA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <img src="https://i.imgur.com/bmF1F2Q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Proceed to install Active Directory on DC-1. Install Active Directory Domain Services and promote DC-1 as a domain controller, setting up a new forest such as “mydomain.com” during the promotion process.
</p>
<br />


<p>
<img src="https://i.imgur.com/LJqtIps.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After installation, restart DC-1 and log back in as user “mydomain.com\labuser” to complete the setup.
</p>
<br />


<p>
<img src="https://i.imgur.com/5sXr5OW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <img src="https://i.imgur.com/nXqvE4x.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create Organizational Units (OUs) for organizing users (employees & admins).
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new employee account named “Jane Doe” with the username “jane_admin” and add jane_admin to the “Domain Admins” Security Group for administrative privileges. Log in as “mydomain.com\jane_admin” for administrative tasks from now on.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, join Client-1 to the domain “mydomain.com.” Set Client-1’s DNS settings to the DC’s Private IP address in the Azure Portal, then restart Client-1 from the portal.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Log in to Client-1 as the original local admin (labuser) and join it to the domain. Verify Client-1’s presence in Active Directory Users and Computers (ADUC) on the Domain Controller.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To enable remote desktop access for non-administrative users on Client-1, log in as jane_admin and allow “domain users” access to remote desktop in system properties. This allows normal users to log into Client-1 remotely.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, create additional users in Active Directory using PowerShell or similar tools. Observe the accounts in the appropriate OU in ADUC.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Test the logins by attempting to log into Client-1 with one of the newly created accounts. This ensures that user accounts are properly set up and accessible within the domain environment.
</p>
<br />
