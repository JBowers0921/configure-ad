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

- Set up Resources in Azure (Active Subscription)
- Ensure Connectivity between Client-1 and Domain Controller
- Install Active Directory
- Create an Organizational Unit, Admin and Normal User Account in AD
- Join Client-1 to your domain
- Set up Remote Desktop for non-admin users on Client-1
- Create additional users and log in with one of the created users

<h2>Deployment and Configuration Steps</h2>

<p>
1.   In order to set up resources in Azure: First create a Resource Group(RG). Then create the first Virtual Machine(VM). Name the VM "DC-1" (be sure to select Image: Windows Server 2022 Datacenter).
<p>    
<img src="https://i.imgur.com/lPWiTAn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />  
<p>  
<img src="https://i.imgur.com/5oxv7mf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
<br />  
<p>  
1.a.   While VM1 (DC-1) is being created, continue with your second VM and name it "Client-1". Repeat the steps in creating the DC1 VM. *Be sure to select Image: Windows 10 Pro, Version.... . Also make sure both VMs are using the same Vnet.
</p>
<br />  
<p>
  
<img src="https://i.imgur.com/kXQhXJ9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br /> 
<br />  
<p>
1.b.   Go back to Home in Azure and search and select the VM Icon. You will see the VMs that you have created similar to the ones displayed below.
</p>
<br />
<br />  
<p>
<img src="https://i.imgur.com/di1lu3l.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />  
<p>  
</p>
1.c.   Before moving on to the next step of ensuring connectivity, go back to DC1 and set the NIC's Private IP address to read "Static" by: Opening DC1 and go to Networking > Network Interface(dc-1164). Click on the letters/numbers to the right (dc-1164)  
<br />  
<p>
</p>
<br />
<p>
<img src="https://i.imgur.com/aGgD7Yc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<br />
<p> 
<br />  
<p>  
 1.c.1   The next screen will show: IP Configurations. Under the Private IP address, click on the numbers to open the next screen.   
<p>
 <br />
<br />  
<p>  
<img src="https://i.imgur.com/3lbRL23.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
<br />
<p> 
<br />  
<p>  
 1.c.2   When the next screen opens, under the word "Assignment", click and change the status to "Static" and save.
<p>  
<p> 
<br />  
<p>  
<img src="https://i.imgur.com/xALMXCH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<br />  
<p>  
2.   In order to ensure connectivity via the "ping" command, Open Client-1 (Azure) and copy the Public IP address. Open a Remote Desktop window and paste Client-1's Public IP address in the field and log into Client-1 with your created credentials.  Next, go back to Azure, into DC-1 and copy the Private IP address. Return to Client-1 Remote Desktop. In the Windows search field, type "Administrator Comman Promp" and open it. Next to your user name, type "ping -t"and DC1's Private IP and hit enter. You will see the request "time out" due to DC1's firewall blocking icmp traffic. 
<br />  
</p>
<br />
<p>
<img src="https://i.imgur.com/Sr6dJxx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<br />  
<p>  
2.a   In order to allow traffic, go back to Azure, into DC1 and copy the Public IP address. Open a separate (2nd) remote desktop and connect to it using DC1's Public IP address. You will see a "Server Manager" Dashboard.
<br />  
<p>  
<br />
<p>
<img src="https://i.imgur.com/EL2Vh1w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />  
<p>  
</p>
<br />
<p>   
<img src="https://i.imgur.com/nhM5oYd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<br />  
<p>  
</p>
<p>
2.a.1    Next, while still in DC1 Remote, go to the Windows search field and type "wf.msc" (windows firewall) and open the App.
<br />  
<p>  
<br />
<p>
<img src="https://i.imgur.com/1tRAczA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <br />  
<p> 
<p>  
</p>
<p>
2.a.2    App opens. Expand the window in order to view all.  In the upper left corner, click on "Inbound Rules". Click on "Protocol" and look for ICMPv4 (2 Core Networking). Highlight and right-click on both(one at a time) and select "Enable Rule".
<br />  
<p>  
<br />
<p>
<img src="https://i.imgur.com/ylpF1vI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
 <br />  
<p> 
</p>
<br />
2.a.3    Minimize DC1 Remote and go back to Client-1 Remote. You should start seeing the "Reply from" message start to populate. This verifies connectivity. Pres "CTRL C" to stop the ping.
<br />  
</p>
<br />
<p> 
<img src="https://i.imgur.com/9N1MpYs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<br />  
</p>
<br />
<p>  
3.   In order to install Active Directory, log into DC-1 Remote Desktop. Go to the Windows search field and type "Active Directory Domain Services". Open and install the app. The Server Manager window will open. Click on "Add roles & Features", click NEXT, NEXT AND NEXT. You will be on the "Select server roles" screen.
</p>
<br />
<br />  
<p>
<p>
<img src="https://i.imgur.com/jd02ltl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<br />  
<p> 
3.a    On the Select server roles screen, click on "Active Directory Domain Service" and then "Add Features". Then on the next screens, click NEXT, NEXT and INSTALL.
</p>
<br />
<br />  
<p>
<p>
<img src="https://i.imgur.com/dDZJJFx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<br />
<br />  
<p>
3.b    On the next screen, wait for the process to finish and click CLOSE. 
<p>
<br />
<br />  
<p>  
<img src="https://i.imgur.com/bFLw62u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
<br />
<br />  
<p>  
3.c   On the next screen, look for the warning triangle by the flag on the dash board tool bar. Click on it to open a small screen. Select " Promote This Server to a domain...". 
<p>
<br />
<br />  
<p>
<img src="https://i.imgur.com/TR0liPc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
<br />
<br />  
<p>
3.d   On the Deployment Configuration screen, select "Add a forest" and then give it a root domain name. Click NEXT.  
<p>
<br />
<br />  
<p>
<img src="https://i.imgur.com/MsTs871.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
<br />
<br />  
<p>  
3.e   On the Domain Controller screen, enter your generic password (Password1 for training). Type it again for confirmation. Then on the proceding screens :Click NEXT, NEXT. When the Net Bios domain populates, click NEXT, and NEXT on Paths, NEXT on Review. Wait on the Precheck, then click INSTALL.  Wait for the Active Directory installation to complete. You may get logged out of DC-1 remote desktop. Log back in with your newly created credentials(anydomain.com\username and then password) using DC-1's Public IP address found in Azure.
<p>
<br />
<br />  
<p>
<img src="https://i.imgur.com/W4yYfxh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<p>
<br />
<br />  
<p>       
4.  In order to create an Organizational Unit, Admin and Normal User Account in AD, while DC-1 is still open go to "Tools" and click on "AD Users and computers".
</p>
<br />
<br />  
<p>
<p>
<img src="https://i.imgur.com/n8UMVu3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />  
<p>  
</p>
<p>
4.a    Next, on the new screen, right-click on your domain name. Go to NEW and on the drop down, select Organizational Unit.              .
</p>
<br />
<br />  
<p>
<p>
<img src="   " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br 
4.b    Next               .
</p>
<br />
<br />  
<p>
<p>
<img src="   " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />  
<p>
<p>
4.c    Next               .
</p>
<br />
<br />  
<p>
<p>
<img src="   " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />  
<p>
<p>   
5.   In order to  Set up Remote Desktop for non-admin users on Client-1:.
</p>
<br />
<br />  
<p>
<p>
<img src="   " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>
6.  Finally, In order to create additional users and log in with one of the created users:.
</p>
<br />
<br />  
<p>
