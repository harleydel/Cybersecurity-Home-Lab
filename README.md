<h1>Cybersecurity Home Lab for Detection and Monitoring</h1>

<h2>Description</h2>
<h3> Overview </h3>
<p> The goal of this project is to create a small Homelab environment used to practice and and improve my skills as a Cybersecurity Analyst. I will be following a guide made by Cyberwox Academy that can be found <a href = "https://cyberwoxacademy.com/building-a-cybersecurity-homelab-for-detection-monitoring/">here</a>. With that said, this writeup will still be in my own words and including my own experiences and learning process throughout the project.</p>

<h3> Content </h3>
<ul>
 <li><a href="#host">My Host PC</a></li>
 <li><a href="#pfsense">Configuring pfSense firewall for Network Segmentation & Security using VMware</a></li>
 <li><a href="#securityonion">Configuring Security Onion as an all-in-one IDS, Security Monitoring, and Log Management Solution</a></li>
 <li><a href="#kali">Configuring Kali Linux as an attack machine</a></li>
 <li><a href="#pfconfig">pfSense Interfaces and Firewall Rules</a></li>
 <li><a href="#windowsserver">Configuring a Windows Server as a Domain Controller</a></li>
 <li><a href="#windowsdesktop">Configuring Windows Desktops</a></li>
 <li><a href="#splunk">Configuring Splunk</a></li>
 <li><a href="#forwarding">Installing Universal Forwarder on Windows Server</a></li>
 <li>Potential Linux machines added for exploitation, detection, or monitoring purposes</li>
</ul>

<h3>Homelab Network Design & Topology</h3>
<img src = "https://static.wixstatic.com/media/1f97f7_c3819a585fb44cc896e93c99d512ba1a~mv2.jpg/v1/fill/w_740,h_496,al_c,q_90/1f97f7_c3819a585fb44cc896e93c99d512ba1a~mv2.webp"/>

<h3><a id = "host">My Host PC</a></h3>
<p>I will be using my PC which I started to build in 2022 and have slowly been upgrading since I got it. The specs are as followed:
 
 <b>CPU</b>: Intel® Core™ i7-12700F Processor<br>
 <b>RAM</b>: XPG GAMMIX D20 DDR4 3600MHz 32GB (4x8GB)<br>
 <b>STORAGE</b>: WD_BLACK 500GB+1TB+1TB SN770 NVMe Internal Gaming SSD Solid State Drive<br>
 <b>GRAPHICS CARD</b>: EVGA GeForce RTX 3070 FTW3 Ultra Gaming<br>
 <b>MOTHERBOARD</b>: ASUS TUF GAMING Z690-PLUS WIFI<br>
 <b>HOST OS</b>: Ubuntu (500 GB) + Windows 11 Home (1000 TB) + Ubuntu (1 TB) *Will be using this third SSD I purchased solely to build
 out my Home Lab and use for future projects!*<br>
</p>

<h3><a id = "pfsense">Configuring pfSense as the firewall</a></h3>

<ol>
 <li>Creating the virtual machine in VMware.</li>
  <ul>
   <li>Keep 20 GB disk size.</li>
   <li>Make sure "split virtual disk into multiple files" is selected.</li>
   <li>Click customize hardware and add 5 network adapters. Note: When I did this and went to choose the "custom" option for each there wasn't enough 
    vmnet options there already. I had to go into the virtual network editor and add a few more, each with the "host-only" configuration. At this step
    you can also remove the sound adapter and usb controller.</li>
  </ul>
 <li>Installing pfSense</li>
  <ol>
   <li>Choose install and accept all defaults until you get to the "Partitioning" screen in which you will go down one and choose "Auto (UFS) BIOS"</li>
   <li>Once pfSense is installed and has rebooted now we will configure the interface IP addresses. (This is all covered in the CyberWox Academy
   walkthrough.)</li> 
   <li>After the IP address configurations are done, pfSense is now installed and the rest of the configuration will happen through the webConfigurator
    in Kali.</li>
  </ol>
</ol>
<a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Installing-and-Configuring-pfSense-Firewall">Click here to view photo documentation for each step of the pfSense process.</a>

<h3><a id = "securityonion">Configuring Security Onion as the IDS, Security Monitoring, and Log Management</a></h3>
<ol>
 <li>Creating the virtual machine in VMware.</li>
  <ul>
   <li>Increase to 200 GB disk size.</li>
   <li>Make sure "store virtual disk as a single file" is selected.</li>
   <li>Click "Customize Hardware" and increase the memory to somewhere between 4-32GB, and add two Network Adapters assigned to Vmnet 4 and Vmnet 5.</li>
  </ul>
 <li>Installing Security Onion</li>
  <ol>
   <li>For the most part you will be accepting defaults for everything but there are a few selections needed to be changed as follows:</li>
   <ul>
    <li>Change from STATIC to DHCP on the management interface window.</li>
    <li>Choose ens35 on the NIC Monitor Interface window.</li>
    <li>Make sure you select NO at the "so allow" window.</li>
   </ul>
  </ol>
 <li>Configuring Security Onion through Ubuntu Desktop</li>
  <ul>
   <li>For this step you will install a new Ubuntu Desktop VM in VMware. I won't go over the details as this is pretty simple and I followed all defaults on installation.</li>
   <li>Once Ubuntu is installed you will run the ifconfig command to identify your IP address.</li>
   <li>You will then go back to security onion and run the "sudo so-allow" command, enter your password, and then when prompted type in the IP address from Ubuntu.</li>
   <li>You can then navigate to the Security Onion IP address on Ubuntu to view the Security Onion Web Interface.</li>
 </ul>
</ol>
<a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Installing-and-Configuring-Security-Onion">Click here to view photo documentation for each step of the SecurityOnion process.</a>

<h3><a id = "kali">Installing Kali Linux to use as the attack machine.</a></h3>
<ol>
 <li>Installing Kali Linux</li>
  <ul>
   <li>For the most part I followed all defaults with installing Kali, so I didn't list all of the details and screenshots. The only <b>important thing to change is the Network Adapter to Vmnet2.</b></li>
   <li>The Kali Linux machine will be used as an attack machine against the Domain Controller, and other machines attached to it. I plan to install machines from VulnHub and other similar sites to practice my red team skills, paired with Security Onion and pfSense for blue team skills.</li>
  </ul>
</ol>

<h3><a id = "pfconfig">pfSense Interfaces and Rules</a></h3>
<ol>
 <li>Accessing the pfSense WebConfigurator</li>
  <ul>
   <li>The pfSense WebConfigurator will be acccessed from Kali Linux to make changes to the pfSense interface and firewall rules.</li>
   <li>The initial step is to load up Kali and access the pfSense WebConfigurator from a browser at the 192.168.1.1 IP. Initially when setting this up I had two issues. The first is that when going to the IP, I was taken to the page for my home router login, rather than pfSense. To fix this I simply had to <b>remove my NAT network adapter from my Kali settings.</b> The next issue was not being able to connect to the IP at all, which I fixed by releasing and renewing my IP on my Kali machine. On a Windows machine, I know that this can be done with ipconfig /release and ipconfig /renew. On Linux I found that the equivalent to this was <b>dhclient -r eth0 to release, and dhclient eth0 to renew</b>, which fixed my issues and allowed me to continue to the WebConfigurator</li>
  </ul>
 <li>Configuring the Interface and Firewall settings using WebConfigurator</li>
 <ol>
  <li>When the pfSense WebConfigurator is accessed, the login is "admin" for username, and "pfSense" for password.</li>
  <li>After clicking through to step 2 of 9, add 8.8.8.8 as the Primary DNS Server, and 4.4.4.4 as the Secondary DNS server.</li>
  <li>At step 3 of 9 you choose your timezone.</li>
  <li>At step 4 of 9 everything is left alone side from unchecking the last two options.</li>
  <li>At step 6 of 9 the admin password is set.</li>
  <li>After clicking "reload" at step 7, the pfSense Wizard is complete and now on to changing the Interface configurations.</li>
  <li>Navigate to LAN under "Interfaces" in the top menu, and change theh Description to Kali and click save.</li>
  <li>Repeat the same process to change the names and Network Ports of each interface to what is shown in the screenshots (link below). For OPT3 which will be changed to "SpanPort", make sure to click the "Enable Interface" checkbox.</li>
  <li>Navigate to "Assignments" under "Interfaces". On this window click "Bridges" > "Add", on this next window select "VICTIMNETWORK" in the "Member Interfaces" menu, and then click "Advanced Configuration" and select "SPANPORT" under the Span Port menu, click "Save" at the bottom of the window.</li>
  <li>Next, navigate to "Rules" under "Firewall" in the top menu. Click the "Add" button with the downward facing arrow. On this page under "edit firewall rule" select "Any" in the "Protocol" dropdown box. Scroll to the bottom and click "Save".</li>
</ol>
</ol>
 
 <a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Configuring-Interfaces-and-Firewall-Settings-through-the-pfSense-WebConfigurator">Click here to view photo documentation for each step of the pfSense configuration process.</a>


<h3><a id = "windowsserver">Configuring Windows Server as a Domain Controller</a></h3>
<ol>
 <li>Creating Windows Server Virtual Machine on VMware</li>
  <ul>
   <li>When installing Windows Server, we will installing with mostly defaults but there are a few things to make sure of.</li>
    <ol>
     <li>When you get to the window asking for product key, simply click "Next" and then "Yes" to continue without a product key.</li>
     <li>At the end of the initial setup, <b>make sure to change the network adapter to VMnet3, and uncheck "Power on Virtual Machine after creation."</b>, then click finish.</li>
     <li>Once all that is done, <b>click "Edit virtual machine settings" and remove the Floppy drive.</b></li>
    </ol>
  </ul>
 <li>Windows Server Installation</li>
  <ol>
   <li>Power on the Windows Server Virtual Machine.</li>
   <li>Click "Install Now" and then "Next". When you get to the window asking for the Windows Server version choose <b>"Windows Server 2019 Standard Evaluation (Desktop Experience)"</b></li>
   <li>Click <b>"Custom: Install Windows only (advanced)". When on the partitioning screen, click "New" > "Apply" > "OK" > "Next"</b> and wait for installation to complete.</li>
   <li>At the final screen, create an admin password and then you should be taken to the Windows Server lock screen.</li>
  </ol>
 <li>Windows Server Configurations</li>
  <ol>
   <li>First thing to do is change the name of the Domain Controller. This can be done by going to settings, searching for "pc name", and then renaming your PC. Once this is done, restart the Domain Controller.</li>
   <li>Once the DC is rebooted, <b>click on "Add Roles and Features" under the "Manage" menu option</b>. Once here, keep clicking "Next" until you reach the <b>"Server Roles" menu. At this menu, select the checkbox for "Active Directory Domain Services", and then "Add Features".</b> Keep clicking "Next" until you're at the confirmation menu, and click "Install".</li>
   <li>Once this installation is done, you will see a flag with a yellow caution triangle in the top menu. Click it and then <b>select "Promote this server to a domain controller". Select "Add a new forest"</b>, specify a domain name, set a password, and continue clicking "Next" until you're at the "Prerequisites Check" menu. Click "Install" and wait for the DC to reboot.</li>
   <li>Once the machine has rebooted, navigate again to the <b>"Add Roles and Features" menu option</b>. On this window, click through until you are at <b>"Server Roles", select the checkbox for "Active Directory Certificate Services", select "Add Features". Click "Next" until you are at the "Confirmation" menu, select the checkbox for "Restart the destination server automatically if required"</b>, select "Yes" then "Install", and after the installation, click "Close".</li>
   <li>On the top menu there will again be a yellow caution symbol next to the flag icon. Click this and then <b>click "Configure Active Directory Certificate Services on the destination server"</b>, click "Next". <b>On the "Role Services" menu, select the checkbox for "Certification Authority".</b> Continue clicking "Next" until you're at the <b>"Validity Period" menu and change it to 99 years.</b> Continue clicking "Next" until you are at the "Confirmation" menu and select "Configure". Once this is done, manually restart the server in order for all settings to take effect.</li>
   <li>We will now create two users for our Windows 10 Systems that will be added in the next step. To do this, on the main menu <b>select "Active Directory Users and Computers" under "Tools".</b> Select your Domain Name > Users, right click and select "New User". <b>Create a new user with any credentials and a password that never expires. Once you have created one, right click on them and select "Copy" and repeat the process to create a second user.</b></li>
   <li>Lastly, we will set pfSense to the default gateway for the Domain Controller. <b>To do this, go to Control Panel > Network and Internet > Network Connections.</b> (For me, I had to search the "Network Connections" menu option. <b>Right click on "Ethernet0" > select "Properties" > right click "Internet Protocol Version 4 (TCP/IPv4) from the checkboxes > select "Properties" > select "Use the following IP address" and configure as following: IP address: 192.168.2.10, Subnet Mask: 255.255.255.0, Default Gateway: 192.168.2.1, Preferred DNS server: 192.168.2.1.</b> (More details in the screenshots found below).</li>
 </ol>
</ol>
 
 <a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Installing-and-Configuring-Windows-Server-2019-as-Domain-Controller">Click here to view photo documentation for each step of the Windows Server configuration process.</a>
 
<h3><a id = "windowsdesktop">Configuring Windows 10 Desktops and Adding a User to the Active Directory Domain</a></h3>
<ol>
 <li>Creating the Windows Desktop Virtual Machines</li>
  <ol>
   <li>Most of the initial settings here will be the same as setting up the Windows Server machine. Start by creating the virtual machine in VMware, accepting all defaults, skipping the product key, changing the network adapter to Vmnet3, unchecking the box to power on after creation, and removing the floppy drive.</li>
   <li>The name for this machine will be the name of the first user created in the previous steps.</li>
   <li>Once Windows is installed on this machine, configure Windows 10 like normal whenever installing Windows on any desktop. Select "I don't have internet", continue with limited setup, and set the first user and password to the same as the settings created in the Domain Controller setup. Uncheck ALL of the privacy settings, choose "Not Now" for Cortana, and then restart the PC."</li>
   <li>Follow all the same steps in setting up a second Windows 10 Desktop Virtual Machine using the credentials of the second user created during the Domain Controller setup.</li>
 </ol>
 <li>Joining the PCs to the Domain</li>
  <ol>
   <li>On the first Windows Desktop machine we just created, navigate through "Network Adapter Settings" to the same "Internet Protocol Version 4 Properties" window that we used to configure the Domain Controller in the previous step.</li>
   <li>Here you will set the following IP settings: <b>IP address: 192.168.2.21, Subnet Mask: 255.255.255.0, Default Gateway: 192.168.2.1, Preferred DNS Server: 192.168.2.10.</b></li>
   <li>After that is all setup, <b>search "Domain" in the windows search bar, and select "Access work or school". Select "Connect" > "Join this device to local Active Directory Domain" > Enter the domain name you set in the Domain Controller step (remember to include .local at the end).</b></li>
   <li><b>At this step you will get an error message which is normal.</b> You now need to navigate to the pfSense WebConfigurator. Once here, navigate to "Services" > "DHCP Server" > "VICTIMNETWORK" > "Other Options" > "Domain Name" and type in the domain name from before. </li>
   <li>Now go back to the Windows Desktop machines and joining the domain should work. You will now enter the Username as "Administrator" and the password you set for your Domain Controller. Select "Skip" > Restart your computer.</li>
   <li>Duplicate all of these steps for the second Windows 10 Desktop machine and this step is done.</li>
 </ol>
</ol>
 
 <a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Installing-Windows-10-Desktop-Machines-and-Joining-to-the-Domain">Click here to view photo documentation for each step of the Windows Server configuration process.</a>
 
 <h3><a id = "splunk">Installing and Configuring Splunk on Ubuntu Server as our SIEM</a></h3>
<ol>
 <li> The first step is to install Ubuntu Server onto VMware. For this step, all defaults are accepted for the most part. <b>The only important things to change are: give at least 80 gb to the Hard Disk (I gave 100 gb), remove the USB controller, sound card, and printer, and then add a second Network Adapter configured with VMnet6.</b></li>
 <li>Once the Ubuntu Server VM is setup, run it and install with all defaults. <b>Make sure to select "Install OpenSSH Server" when asked.</b></li>
 <li>Once everything is installed, and you have rebooted the machine, login with the credentials you created during installation.</li>
 <li>We will now install a GUI on our Ubuntu Server. To do this type <b>"Sudo apt install tasksel". Once that is done type "Sudo apt install ubuntu-desktop".</b> When that is finished installing, reboot the machine.</li>
 <li>Once the machine is rebooted, it should start up with a GUI rather than terminal only. Log in and click through the typical installation defaults for ubuntu. Open a web browser (my installation didn't come with one so I installed Firefox) and navigate to splunk.com. At the top right corner <b>click "Free Splunk"</b>, and if you do not already have a Splunk account, create one. Once that is done, navigate to Products > Free Trials & Downloads > Splunk Enterprise Free Trial. <b>Under "Choose your installation package", choose Linux, and download the .tgz version of Splunk Enterprise.</b></li>
 <li>Once Splunk is downloaded, go back to the terminal and cd to the Downloads directory. <b>Untar the file with the command "tar xvzf [name of download file]".</b> Next navigate to the splunk/bin directory and <b>start splunk with the command "./splunk start".</b>Next, scroll to the bottom of the readme doc and select yes, and then create a username and password and wait for the splunk interface to finish starting up.</li>
 <li>Now navigate to the splunk instance on your browser by going to "http://splunk:8000" and login.</li>
</ol>
 
 <a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Installing-and-Configuring-Splunk-on-Ubuntu-Server-as-our-SIEM">Click here to view photo documentation for each step of the Splunk configuration process.</a>
 
  <h3><a id = "forwarding">Installing Universal Forwarded on Windows Server</a></h3>
<ol>
 <li>The final step is to configure the Universal Forwarder to be able to send Windows Event Logs to Splunk. To do this, first start up your Splunk instance and log in through the browser.</li>
 <li>When logged in, navigate to Settings > Forwarding and Receiving > Configure Receiving > Click "New Receiving Port". <b>Set "Listen on this port" to port 9997</b>, and click "save".</li>
 <li>Next, go to Settings > Indexes > New Index, for "Index Name" name it "wineventlog", leave everything else alone and click save.</li>
 <li>Now open the Windows Server Domain Controller and open internet explorer. Go to Settings > Internet Options > Custom Level, and enable downloads. I now used Internet Explorer to download Google Chrome for the Universal Forwarder download (since we all know how horrible Internet Explorer is). <b>One issue I had here is initially my Windows Server not having any internet connection. To fix this, I went to pfSense WebConfigurator through Kali then navigated to Firewalls > Rules > VictimNetwork > Click "Add" with the option of the downward pointing arrow. On the next screen, change Action > Pass, Protocol > Any, Source > Any, and Destination > Any.</b> After doing this I went back to the Domain Controller and had internet connection.</li>
 <li>Once Chrome is installed, go to Google and search for "Universal Forwarder", go to the Splunk website and login. Once logged in go to Products > Free Trials & Downloads > Universal Forwarder, and download the .msi for Windows Server.</li>
 <li>Once that is downloaded, double click to open the installer. On the first window, click the checkbox to accept the License Agreement, then click next. Create a username and password, click next. <b>For the Deployment Server Hostname or IP, this will be the IP of your Splunk interface, with the port 8089. For the Receiving Indexer, this will be the same IP with port 9997.</b> Click Install.</li>
 <li>Next you will head back to your Splunk interface, and on the browser navigate to Settings > Add Data > Forwarder. On this screen, under "Available Hosts", you should see your Domain Controller machine, click on it and it will populate into "Selected Hosts", set the "New Server Class Name" to "Domain Controller", click next. On the next window, click "Local Event Logs" and choose all of the available event logs. Here, you can choose to configure any of the other desired settings you want to monitor. I may go back and look deeper into this in the future, but for this initial setup I only chose the Local Event Logs, click next. On the next window next to "Index", click the dropdown and choose "wineventlogs", click review, click submit.</li>
 <li>At this point all of the initial setup for the Home Lab is complete!</li>
</ol>
 
 <a href = "https://github.com/harleydel/Cybersecurity-Home-Lab/wiki/Forwarding-Windows-Event-Logs-to-Splunk-with-the-Universal-Forwarder">Click here to view photo documentation for each step of the Splunk configuration process.</a>
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
