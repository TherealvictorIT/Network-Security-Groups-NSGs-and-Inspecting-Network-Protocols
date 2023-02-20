<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we will be observing various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure [Virtual Machines (VM]
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Testing Connectivity Between Hosts</h2>
<p>
</p>
<p>
Welcome to my tutorial on Network Security Groups and Inspecting Network Protocols. To start off we will create two VMs on Azure. One machine will be running on Linux while the other will be running on Windows 10. Both will be running on the same VNet. 
</p>
On our Windows machine we will download Wireshark to see the actual live traffic that is happening on our virtual machine. Wireshark download Link: https://www.wireshark.org/download.html  
</p>
<br />
<p>
<img src="https://i.imgur.com/VZS81An.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once installed open Wireshark and filter for Internet Control Message Protocol (ICMP) Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol (ping tests connectivity between hosts). When we filter wirehsark to only capture ICMP packets and ping the private IP address of our linux machine we can visually see the packets on wireshark.
</p>
<br />
<p>
<img src="https://i.imgur.com/K7Ql4T0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>  
We can also inspect individual packets and see the actual data that is being sent in each ping. 
</p>
<p> 
<h2>Blocking inbound IMCP Traffic Using a Firewall</h2>
<p>
</p>
<p>
<img src="https://i.imgur.com/RDFOlPM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Next we will perpetually ping the Linux machine with the command ping -t on powershell on our Windows VM. This will continually ping the machine until we decide to stop it. 
<p>
<img src="https://i.imgur.com/51kTugI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
On our Linix machine we will block inbound ICMP traffic on it's firewall. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP.
</p>
<img src="https://i.imgur.com/vcEweiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>  
Once we do that we will stop recieving echo replys from the Linux machine on our Windows machine.
  
<h2>Exploring SSH Traffic</h2>

Next we will use our Windows machine to Secure Shell (SSH) to our Linux machine. SSH is often used to "login" and perform operations on remote computers but it may also be used for transferring data. SSH has no graphical user interface (GUI) it just gives the user access to the machines command-line interface (CLI). 
<p>
<img src="https://i.imgur.com/zkw58Q5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
We will set the wireshark filter to capture SSH packets only. When we SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets. We can use commands like "uname -a" to tell us about the operating system (OS) running. or "ls -lasth" to list folders and files in the current directory and much more
<h2>Requesting new IP address using DHCP</h2>
</p>
<br />
Now we will use wireshark to filter for Dynamic Host Configuration Protocol (DHCP) this works on ports 67/68, it is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/sf85UL7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Finding Webpage IP Address</h2>
We can also filter by Domain Name System (DNS) "nslookup" traffic. We will set wireshark to filter DNS traffic. 
</p>
<br />
<img src="https://i.imgur.com/G3Udqw8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
<p>
<h2>Filtering by RDP Traffic</h2>
Lastly we will filter for Remote Desktop Protocol (RDP) traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/Q3i1qeo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
