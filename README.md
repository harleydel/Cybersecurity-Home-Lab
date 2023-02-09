<h1>Cybersecurity Home Lab for Detection and Monitoring</h1>

<h2>Description</h2>
<h3> Overview </h3>
<p> The goal of this project is to create a small Homelab environment used to practice and and improve my skills as a Cybersecurity Analyst. I will be following a guide made by Cyberwox Academy that can be found <a href = "https://cyberwoxacademy.com/building-a-cybersecurity-homelab-for-detection-monitoring/">here</a>. With that said, this writeup will still be in my own words and including my own experiences and learning process throughout the project.</p>

<h3> Content </h3>
<ul>
 <li><a href="#host">My Host PC</a></li>
 <li><a href = "#pfSense">Configuring pfSense firewall for Network Segmentation & Security using VMware</a></li>
 <li>Configuring Security Onion as an all-in-one IDS, Security Monitoring, and Log Management Solution</li>
 <li>Configuring Kali Linux as an attack machine</li>
 <li>Configuring a Windows Server as a Domain Controller</li>
 <li>Configuring Windows Desktops</li>
 <li>Configuring Splunk</li>
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

<h3><a id = "pfSense">Configuring pfSense</a></h3>
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
