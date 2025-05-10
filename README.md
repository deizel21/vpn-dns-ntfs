# vpn-dns-ntfs
Here is a guide on configuring VPN's, DNS, and NTFS in Microsoft Azure
<p align="center">

  ![image](https://github.com/user-attachments/assets/eb7429d9-d298-4055-ac5a-2f09ee472195)

</p>

<h1>On-premises VPN Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises VPN, DNS and NTFS within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

- <p>

![image](https://github.com/user-attachments/assets/aab28561-619e-43ae-b5c0-d8d01ffae294)


</p>

<p>

  <h2>Setup Proton VPN Lab</h2>
1.	Browse to https://whatismyipaddress.com/ FROM WITHIN YOUR OWN MACHINE and take note of this in a text file
2.	Create a Resource Group
3.	Create a Windows 10 Virtual Machine in another geographic location (try a different country)
a.	Log into the VM with Remote Desktop
b.	Browse to https://whatismyipaddress.com/ and take note of this in a text file

(Sign up for ProtonVPN and test the VPN connection)
4.	On your actual computer, sign up for the free version of Proton VPN https://account.protonvpn.com/signup?plan=free&language=en  
5.	Back within your VM, download the Proton VPN client
a.	Login to the VPN (https://account.protonvpn.com/login) and choose a VPN server in yet another country (such as Japan)
b.	Browse to https://whatismyipaddress.com/  and take note of this in a text file
6.	Try browsing to Google, Disney, and/or Amazon and see if there is anything different about the sites in relation to the location of your VPN server. For example, the language or URL may be different

![image](https://github.com/user-attachments/assets/02c6739c-f612-4937-b31b-beaa3183c176)
![image](https://github.com/user-attachments/assets/404e773e-33b6-496b-8c25-fbc610e541bb)



(Clean up Azure resources)
7.	Delete the resource group you created in Step 2
8.	Ensure the resources/Resource Group has been deleted


  <h2>DNS Lab </h2>

  ![image](https://github.com/user-attachments/assets/6b160201-7e75-4ace-a9e1-15004ee1d644)

  •	Overall Summary of the Lab

Turn on the DC-1 and Client-1 VMs in the Azure Portal if they are off.

A-Record Exercise
1.	Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)
2.	Connect/log into Client-1 as an admin (mydomain\jane_admin)
3.	From Client-1 try to ping “mainframe” notice that it fails
When your computer tries to interact with any hostname on the network it will check the local DNS cache first(fastest), then local Host file (faster) and then DNS Server(slowest).
4.	Nslookup “mainframe” notice that it fails (no DNS record)
5.	Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address. Search Windows Administrative Tools and select the DNS icon > then click DC-1 > then click > click Forward lookup zones > click mydomain.com > right click in space and select New Host (A or AAAA) option > type mainframe >Type Ip address as 10.0.0.4 > click apply> Ok
6.	Go back to Client-1 and try to ping it. Observe that it works

![image](https://github.com/user-attachments/assets/c14e238c-1985-4f55-9905-8e6f84aa88f2)

![image](https://github.com/user-attachments/assets/51f5e791-dfcb-4101-9445-5e38d84d7def)

![image](https://github.com/user-attachments/assets/8d17454a-4785-4173-9cf9-589ac1bcf5ab)


• Local DNS Cache Exercise
7.	Go back to DC-1 and change mainframe’s record address to 8.8.8.8

8.	Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address

9.	Observe the local dns cache (ipconfig /displaydns)

10.	Flush the DNS cache (ipconfig /flushdns).

11.	Observe that the cache is empty (ipconfig /displaydns)

12.	Attempt to ping “mainframe” again. Observe the address of the new record is showing up

![image](https://github.com/user-attachments/assets/ccbe0361-26ac-4b01-b5e8-587f1aaf1b88)

![image](https://github.com/user-attachments/assets/ba334487-add7-420a-ac54-89e13e1256bd)




• CNAME Record Exercise
13.	Go back to DC-1 and create a CNAME record that points the host “search” to “www.google.com”

14.	Go back to Client-1 and attempt to ping “search”, observe the results of the CNAME record

15.	On Client-1, nslookup “search”, observe the results of the CNAME record

![image](https://github.com/user-attachments/assets/232f9a39-ce0b-4900-b2e6-8eb949c77920)

• Finish the lab, but do not delete the VMs in Azure. We will use them for upcoming labs.
If you are done for the day and want to save money, simply “Stop”/turn off the VMs within the Azure Portal



  <h2>NTFS Lab </h2>

![image](https://github.com/user-attachments/assets/96077e88-9705-45de-90e6-5998695f6ada)

![image](https://github.com/user-attachments/assets/4ff2073a-f23b-4666-a380-9af0aafadc72)


• Overall Summary of the Lab
Turn on the DC-1 and Client-1 VMs in the Azure Portal if they are off.

• Create some sample file shares with various permissions
1.	Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)

2.	Connect/log into Client-1 as a normal user (mydomain\<someuser>)

3.	On DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”

4.	Set the following permissions (share the folder)

a.	Folder: “read-access”, Group: “Domain Users”, Permission: “Read”

b.	Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”

c.	Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”

d.	(Skip accounting for now)

• Attempt to access file shares as a normal user
5.	On Client-1, navigate to the shared folder (start, run, \\dc-1)

6.	Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?

• Create an “ACCOUNTANTS” Security Group, assign permissions, an test access
7.	Go back to DC-1, in Active Directory, create a security group called “ACCOUNTANTS”

8.	On the “accounting” folder you created earlier, set the following permissions:

9.	a.	Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”

10.	On Client-1, as  <someuser>, try to access the accountants folder. It should fail. 

11.	Log out of Client-1 as  <someuser>

12.	On DC-1, make <someuser> a member of the “ACCOUNTANTS”  Security Group

13.	Sign back into Client-1 as <someuser> and try to access the “accounting” share in \\DC-1\ - Does it work now?


![image](https://github.com/user-attachments/assets/f2f4a0ba-9798-4e09-a64b-66903d244466)

![image](https://github.com/user-attachments/assets/aca31282-25c6-428f-8784-9a4c9c020044)


This is the end of the Lab.


</p>
<br />
