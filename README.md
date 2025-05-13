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


<h2>Deployment and Configuration Steps</h2>

<p>

![DNS Configuration - Before and After](https://github.com/user-attachments/assets/9f0208e2-440a-4011-b1f5-e55b48860e3a)
</p>
<p>
 We begin by creating two virtual machines (VMs): one named DC-1, running Windows Server 2022, and another named Client-1, running Windows 10. Both VMs are connected to the same virtual network (VNet). After setting up the VMs, we configure the domain controller (DC-1) with a static IP address instead of a dynamic one, and setup DC-1 static IP address as Client-1 main DNS server. This ensures that it can reliably provide Active Directory services to the client machine (Client-1).
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/533cba35-1d81-43d7-ab4a-4518652f6893)
</p>
<p>
We begin by creating two virtual machines (VMs): one named DC-1, running Windows Server 2022, and another named Client-1, running Windows 10. Both VMs are connected to the same virtual network (VNet). After setting up the VMs, we configure the domain controller (DC-1) with a static IP address instead of a dynamic one. This ensures that it can reliably provide Active Directory and DNS services to the client machine (Client-1).
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/bbdd7a30-2746-4192-af7b-5407c075fcef)
![image](https://github.com/user-attachments/assets/098bc3ea-5fc0-42b0-ac27-5fdf508526f9)
</p>
<p>
Next, we connect to the domain controller (DC-1) using Remote Desktop (RDP). Once logged in, we disable the firewall for the domain, private, and public network profiles. This ensures that Client-1 can successfully ping DC-1’s private IP address later in the tutorial without any issues.
</p>
<br />

<p>

![DNS Settings Configuration Screenshot](https://github.com/user-attachments/assets/f23149e4-c8e7-4ea3-8efd-123608fc880e)
</p>
<p>
Next, we’ll update the DNS settings for Client-1 in the Azure portal, setting its DNS server to the static private IP address of DC-1 and then restart Client-1 VM.
</p>
<br />

<p>

![Verifying DNS Configuration on Client-1](https://github.com/user-attachments/assets/7ee62128-63f5-473f-9f01-c7cbcd86ad9b)
</p>
<p>
Once both VMs have restarted, we access the Client-1 VM using Remote Desktop (RDP). In PowerShell, we ping DC-1’s private IP address to confirm that the two machines can communicate over the network. We then run the ipconfig /all command on Client-1 to verify that DC-1’s static IP address is correctly set as the DNS server
</p>
<br />

</p>

![image](https://github.com/user-attachments/assets/ea77dad9-4ce5-4e12-9439-2ecd125fa443)
</p>
<p>
Next, on the domain controller (DC-1), we use Server Manager to install the Active Directory Domain Services.
</p>
<br />

</p>

![Promoting the Server to a Domain Controller](https://github.com/user-attachments/assets/4889a11b-135c-45c2-a4e0-0ef8e2d3278f)
![image](https://github.com/user-attachments/assets/606a45a8-d777-492b-86be-cf3fc270a806)
</p>
<p>
We then promote DC-1 to a Domain Controller and set up a new forest with the domain name called “mydomain.com”.
</p>
<br />

</p>

![Creating Organizational Units in Active Directory](https://github.com/user-attachments/assets/f30a350f-94de-41a0-90a6-0a45e3001054)
![image](https://github.com/user-attachments/assets/d65cc7d5-8c9d-482f-8d5a-187f73e6e28a)
</p>
<p>
Once the DC-1 VM has restarted, we log back in as “mydomain.com\labuser”. Then, we open Active Directory Users and Computers and create two organizational units (OUs) named _EMPLOYEES and _ADMINS.
</p>
<br />

<p>

![Organizational Units Successfully Created](https://github.com/user-attachments/assets/ee5778a2-38ef-4532-87dd-5a6ff672d920)
![image](https://github.com/user-attachments/assets/e4d8e285-d994-434d-9b57-e589e1f49577)
</p>
<p>
In the _ADMINS organizational unit (OU), we create a new user named Jane Doe with the username “jane_admin”. After creating the account, we right-click the user, select Properties, and add her to the Domain Admins security group.
</p>
<br />

<p>
  
![Adding a User to the Domain Admins Group](https://github.com/user-attachments/assets/5028f04a-9ade-4be2-8e8d-bfdb10dc3494)
</p>
<p>
Now, we log in to the Client-1 VM and join it to the domain. To do this, we go to the System settings, click on Rename this PC (Advanced), then select Change. Choose Domain and type mydomain.com. 
</p>
<br />

<p>
  
![Joining Client-1 to the Domain](https://github.com/user-attachments/assets/47ec530e-4b4f-4a3c-a1f2-1a309a1146b2)
![image](https://github.com/user-attachments/assets/6b1f2049-97cd-48b3-a6c5-c2b0feef9ee0)
</p>
<p>
Client-1 is now successfully joined to the domain. Next, we’ll configure Remote Desktop access for non-administrative users. To do this, log into Client-1 with an administrator account and open the System Properties. Under the Remote Desktop tab, enable Remote Desktop and add the Domain Users group to the list of allowed remote users. Once this is set, standard domain users should be able to connect to Client-1 via Remote Desktop.
</p>
<br />

<p>

![Screenshot (1630)](https://github.com/user-attachments/assets/4a0d9bcd-b957-4d61-842c-d43213ebcee2)
![PowerShell Script to Bulk Create AD Users](https://github.com/user-attachments/assets/2eb65b96-2d1a-48f6-bcf6-c289257e1692)
</p>
<p>
Log in to DC-1 as the jane_admin user. Using PowerShell, we’ll run a script that automatically generates thousands of test user accounts in Active Directory. Once the users are created, we’ll  open Active Directory Users and Computers, select one of the new users, and then log into Client-1 using that account to verify domain access.
</p>
<br />

<p>

![Configuring Account Lockout Policy in Group Policy](https://github.com/user-attachments/assets/d30efd89-4e1a-464a-b393-4029c333eb11)
</p>
<p>
Next, we’ll configure account lockout settings for the newly created users. Log in to DC-1 and open Group Policy Management. Expand the mydomain.com domain, right-click on the Default Domain Policy, and select Edit. In the Group Policy Editor, navigate to: Configuration>Policies>Windows Settings>Security Settings>Account Policies>Account Lockout Policy. And set the policy to lock a user account after 5 failed login attempts.
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/1687aa8b-3178-4a04-8718-56086a6697f7)
</p>
<p>
When attempting to log in to Client-1 using one of the newly created accounts, the account becomes locked after five failed login attempts. To unlock the account, we log in to DC-1, open Active Directory Users and Computers, right-click on mydomain.com, select Find, enter the username of the locked account, and click on it to view its properties.
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/4e404aa3-7d1a-4312-bf68-af3a72698e4e)
</p>
<p>
Lastly, if you need to reset a password or disable an account, simply right-click the user account in Active Directory Users and Computers.
