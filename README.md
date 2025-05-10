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
</p>
<p>
In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because its offering Active Directory services to the client machine. Client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the DC as its DNS server. 
</p>
<br />




![image](https://github.com/user-attachments/assets/533cba35-1d81-43d7-ab4a-4518652f6893)





</p>
<p>
We begin by creating two virtual machines (VMs): one named DC-1, running Windows Server 2022, and another named Client-1, running Windows 10. Both VMs are connected to the same virtual network (VNet). After setting up the VMs, we configure the domain controller (DC-1) with a static IP address instead of a dynamic one. This ensures that it can reliably provide Active Directory and DNS services to the client machine (Client-1).
</p>
<br />

<p>

</p>

![image](https://github.com/user-attachments/assets/bbdd7a30-2746-4192-af7b-5407c075fcef)
![image](https://github.com/user-attachments/assets/098bc3ea-5fc0-42b0-ac27-5fdf508526f9)





</p>
<p>
Next, we connect to the domain controller (DC-1) using Remote Desktop (RDP). Once logged in, we disable the firewall for the domain, private, and public network profiles. This ensures that Client-1 can successfully ping DC-1’s private IP address later in the tutorial without any issues.














![DNS Settings Configuration Screenshot](https://github.com/user-attachments/assets/f23149e4-c8e7-4ea3-8efd-123608fc880e)


<p>
Next, we’ll update the DNS settings for Client-1 in the Azure portal, setting its DNS server to the static private IP address of DC-1 and then restart Client-1 VM.
</p>


![Verifying DNS Configuration on Client-1](https://github.com/user-attachments/assets/7ee62128-63f5-473f-9f01-c7cbcd86ad9b)


<br />
</p>
Once both VMs have restarted, we access the Client-1 VM using Remote Desktop (RDP). In PowerShell, we ping DC-1’s private IP address to confirm that the two machines can communicate over the network. We then run the ipconfig /all command on Client-1 to verify that DC-1’s static IP address is correctly set as the DNS server
</p>

![image](https://github.com/user-attachments/assets/ea77dad9-4ce5-4e12-9439-2ecd125fa443)



<br />
</p>

Next, on the domain controller (DC-1), we use Server Manager to install the Active Directory Domain Services.
</p>


![Promoting the Server to a Domain Controller](https://github.com/user-attachments/assets/4889a11b-135c-45c2-a4e0-0ef8e2d3278f)
![image](https://github.com/user-attachments/assets/606a45a8-d777-492b-86be-cf3fc270a806)





</p>
<p>
We then promote DC-1 to a Domain Controller and set up a new forest with the domain name called “mydomain.com”.

<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
