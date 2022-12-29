<h1>Cybersecurity Home Lab for Detection and Monitoring</h1>

<h2>Description</h2>
<h3> Overview </h3>
<p> The goal of this project is to create a small Homelab environment used to practice and and improve my skills as a Cybersecurity Analyst. I will be following a guide made by Cyberwox Academy that can be found <a href = "https://cyberwoxacademy.com/building-a-cybersecurity-homelab-for-detection-monitoring/">here</a>. With that said, this writeup will still be in my own words and including my own experiences and learning process throughout the project.</p>
<h3> Content </h3>
<ul>
 <li>My Host PC</li>
 <li>Configuring pfSense firewall for Network Segmentation & Security using VirtualBox</li>
 <li>Configuring Security Onion as an all-in-one IDS, Security Monitoring, and Log Management Solution</li>
 <li>Configuring Kali Linux as an attack machine</li>
 <li>Configuring a Windows Server as a Domain Controller</li>
 <li>Configuring Windows Desktops</li>
 <li>Configuring Splunk</li>
 <li>Potential Linux machines added for exploitation, detection, or monitoring purposes</li>
</ul>
<h3>Homelab Network Design & Topology</h3>
<img src = "https://static.wixstatic.com/media/1f97f7_c3819a585fb44cc896e93c99d512ba1a~mv2.jpg/v1/fill/w_740,h_496,al_c,q_90/1f97f7_c3819a585fb44cc896e93c99d512ba1a~mv2.webp"/>
<h3>My Host PC</h3>
<p>I will be using my PC which I started to build in 2022 and have slowly been upgrading since I got it. The specs are as followed:
 
 <b>CPU</b>: Intel® Core™ i7-12700F Processor<br>
 <b>RAM</b>: XPG GAMMIX D20 DDR4 3600MHz 32GB (4x8GB)<br>
 <b>STORAGE</b>: WD_BLACK 500GB+1TB SN770 NVMe Internal Gaming SSD Solid State Drive<br>
 <b>GRAPHICS CARD</b>: EVGA GeForce RTX 3070 FTW3 Ultra Gaming<br>
 <b>MOTHERBOARD</b>: ASUS TUF GAMING Z690-PLUS WIFI<br>
 <b>HOST OS</b>: Ubuntu (500 GB) + Windows 11 Home (1000 TB) *The Ubuntu OS will be used for this project*<br>
</p>
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
