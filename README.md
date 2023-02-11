<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
</br>
</br>
<h3 align="center">
  Set up your virtual environment
</h3>
</br>
<p>
  Create a Resource Group:
</p>
<p>
<img src="https://i.imgur.com/UFxZ0As.png" alt="RG-Networking"/>
</p>
<p>
Create a Windows virtual machine.
</p>
<p>
  While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Set your username and password in the Administrator Account section as well.
</p>
<p>
<img src="https://i.imgur.com/u4ypcUS.png" alt="Windows VM1"/>
<img src="https://i.imgur.com/UfayPWK.png" alt="Username,Password"/>
</p>
<p>
Create an Ubuntu (Linux) virtual machine.
</p>
<p>
  Repeat the same steps above with the Resource Group, Virtual Network, username, and password for the Ubuntu Linux VM.
</p>
<p>
 <img src="https://i.imgur.com/KjlvF9c.png" alt="Linux VM2"/>
 <img src="https://i.imgur.com/T1lddsX.png" alt="Linux Username,Password"/>
</p>
<p>
Observe Your Virtual Network within Network Watcher:
</p>
<p>
  <img src="https://i.imgur.com/H9A9kYu.png" alt="Network Topology"/>
</p>
<br />
<br />
<h3 align="center">
  Observe ICMP traffic
</h3>
<br />
<p>
  Remote Desktop into the Windows 10 Virtual Machine, install Wireshark, open it and filter for ICMP traffic only.
</p>
<p>
   <img src="https://i.imgur.com/iDI58TZ.png" alt="Remote Desktop"/>
   <img src="https://i.imgur.com/rkspfsF.png" alt="Wireshark"/>
</p>
<p>
Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark:
</p>
<p>
 <img src="https://i.imgur.com/OPLia0u.png" alt="Linux Private IP"/>
 <img src="https://i.imgur.com/0Hv2qUl.png" alt="ICMP Ping"/>
</p>
<p>
  Attempt to ping a public website (www.google.com) and observe the traffic in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/bgs0Du7.png" alt="Ping Google"/>
</p>
<p>
  Initiate a perpetual/non-stop ping from the Windows 10 VM to the Linux Ubuntu VM:
</p>
<p>
  <img src="https://i.imgur.com/E4vZst0.png" alt="Perpetual Ping"/>
</p>
<p>
  Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic, while back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity:
</p>
<p>
  <img src="https://i.imgur.com/JQXLntq.png" alt="Deny ICMP"/>
  <img src="https://i.imgur.com/F10qMVh.png" alt="ICMP Traffic Stopped"/>
</p>
<p>
  Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM and back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity should start working again. Finally, stop the ping activity (control + c):
</p>
<p>
  <img src="https://i.imgur.com/n2ki9UE.png" alt="Allow ICMP"/>
</p>
<br />
<br />
<h3 align="center">
  Observe SSH traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for SSH traffic only and from your Windows 10 VM, “SSH into” your Ubuntu virtual machine (via its private IP address). Type commands (ls, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark.
</p>
</p>
  Exit the SSH connection by typing ‘exit’ and pressing [return]:
</p>
  <img src="https://i.imgur.com/MVsNcNV.png" alt="SSH Into Linux"/>
<p>
<br />
<br />
<h3 align="center">
  Observe DHCP Traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
</p>
Observe the DHCP traffic appearing in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/2u9zIsW.png" alt="ipconfig /renew"/>
</p>
<br />
<br />
<h3 align="center">
  Observe DNS traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for DNS traffic only.
</p>
<p>
  From your Windows 10 VM within a command line, use nslookup to see what google.com's IP addresses are and observe the DNS traffic being shown in WireShark:
</p>
<p>
  <img src="https://i.imgur.com/bd9jOQh.png" alt="nslookup"/>
</p>
<br />
<br />
<h3 align="center">
  Observe RDP traffic 
</h3>
<br />
<p>
  Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
</p>
<p>
  Oserve the immediate non-stop spam of traffic? Why is it non-stop spamming vs only showing traffic when a command is inputted?
</p>
<p>
  The answer is because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted:
</p>
<p>
  <img src="https://i.imgur.com/8ITLfVN.png" alt="TCP Port Traffic"/>
</p>
<br />
<br />
<p>
 Now that we're done, close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion to avoid incurring unnecessary extra costs.
</p>
