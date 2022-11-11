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
4.a    Next, on the new screen, right-click on your domain name. Go to NEW and on the drop down, select Organizational Unit. On the next screen, name the Org Unit folder "_Employees" and click OK. Repeat the steps and create an "_Admin" Org Unit folder. Right-click on the domain name and "refesh" to see the changes.            .
</p>
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img923/451/I8FLO2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />  
<p>
<p>  
<img src="https://imagizer.imageshack.com/img923/6541/sNtpE2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
4.b    In order to create a new User, right-click on your "_Admin" OU folder. Go to NEW and User and fill in the fieds as shown below and then click NEXT:
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img924/2533/XE8iOt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br
<br />     
</p>
4.c    On the next screen, set the password and for training, uncheck "user must change password". Click NEXT and FINISH.   
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img922/3686/lnnofK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />  
<p>
<p>
4.d   Now, you will need to make Jane Doe a Domain Admin and assign her to the Domain Admin Group.     .
</p>
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img923/9737/7Z0eWn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />  
<p>
 4.e   To assign Jane Doe to the Domain Admin group, right click on jane doe's domain name and go to PROPERTIES > MEMBERS OF > ADD.
</p>
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img922/1315/vlHyp1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />  
<p>
<p> 
 4.f   Next screen: In the "Enter the object names.." field, type the word "Domain" and click "check Names". Options will populate. Selct "Domain Admins", then OK, APPLY, and OK.
</p>
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img924/4563/UY8ccZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />
<p>
<p> 
4.f.1    To see the results, log out of DC. Log back in as yourdomain.com\jane_admin and the password (Password1). To insure connection, go to the command prompt and open it and you can see "c:\user\jane_admin".
</p>
<br
<br />
<p>
<img src="https://imagizer.imageshack.com/img922/4377/uxmhgm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<br
<br />
<img src="https://imagizer.imageshack.com/img922/7079/hhg893.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<br
<br />
<p>
  
5.   In order to join Client-1 to your domain controller (DC-1): You should first return to Azure and get DC-1's Private IP Address (Write it down).  While still in Azure, open Client-1 VM. Go to : Networking > Network Interface Client 1685 (yours may be differnt) and click on "Client 1685". Next screen, scroll down and click on "DNS Servers". Then Change it to "Custom" and add DC-1's Private IP address (exam: 10.0.0.4) then click SAVE. Wait for the updates to take place. Once the updates are complete, go back to Azure and RESTART Client-1 (VM).
</p>
<br />
<br />  
<p>
<img src="https://imagizer.imageshack.com/v2/679x586q90/923/hGGjQM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br
<br />
<p> 
5.a  Next, return to Azure and set Client-1's DNS settings to DC-1's Private IP address by: logging into Client-1 via Remote Desktop using the Public IP address and with your original user name and password. Once in remote desktop-Client-1, Go to Windows Start >Search Settings > About > "Rename this PC Advanced". Next window, click on "Change". Next small window: Select "Domain", add your domain name and click OK. Next small pop up window, add "yourdomain.com\jane_admin" and the password. Click OK.   
</p>
<br />  
<p>
<p>   
<img src="https://imagizer.imageshack.com/img922/637/KW1CAh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>  
 5.b  You will see a small pop up welcome screen (possibly behind the other open screens). On that screen, click "OK" and then "OK" again to restart the computer. 
</p>
<br />  
<p>
<p>   
<img src="https://imagizer.imageshack.com/img924/6769/WOGFgT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p> 
5.c  Log back into Remote Desktop Client-1 as jane_admin.
</p>
<br />  
<p>
<p> 
<img src="https://imagizer.imageshack.com/img922/176/qsAF6l.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img924/416/33cs9j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
<br />  
<p>  
6.   The next step is set up Remote Desktop for non-admin users on Client-1: Go to the Windows Icon at the bottom left of your screen. Right-click on it and go to SYSTEM > REMOTE DESKTOP > under User accounts select "Select users that can remotely access...". Next pop-up window, select ADD. Next pop-up window in the open field, type in "domain users" and click "check names". Next, click OK and OK. Now, all non-admins/ domain users are allowed to connect remotely to Client-1.
</p>
<br />
<br />  
<p>
<p>
<img src="https://imagizer.imageshack.com/img923/6373/fdtoXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>
7.  Finally, In order to create additional users and log in with one of the created users: Log into DC-1 as "jane_admin". Go to the Windows start icon and search for POWERSHELL_ise. Click on it and open as AN ADMIN. Open a new file by clicking on the first document icon under the word "file" on Windows Powershell ISE window.
</p>
<br />
<br />  
<p>
<img src="https://imagizer.imageshack.com/img924/7867/41z8cp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>
7.a  Go to your training document link that was provided on Lab 5 simple list . Open it and copy the "raw data" code and paste the copied contents of the script into the open file on Windows Powershell. You can go to line 3 on the script and adjust the numbers if you like. 
</p>
<br />
<br />  
<p>
<img src="https://imagizer.imageshack.com/img922/6141/zvgZan.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>
7.b  Click on the green triangle at the top to "RUN SCRIPT". You will start to see the traffic created at the bottom.
</p>
<br />
<br />  
<p>
<img src="https://imagizer.imageshack.com/img922/6505/SDk0vV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />  
<p>
<p>   
