<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

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

- Step 1: Create an Azure environment with a Windows 10 and an Ubuntu VM in the same Virtual Network .
- Step 2: Use Wireshark to capture and observe ICMP (ping) traffic between the two VMs and to the public internet .
- Step 3: Configure the Network Security Group (NSG) to create a firewall rule that blocks ICMP traffic, and then re-enable it .
- Step 4: Inspect various network protocols (SSH, DHCP, DNS, and RDP) in Wireshark to observe their unique behaviors .

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 1: Create Azure VM Environment</h2>
First, in the Azure portal, create a new Resource Group. Deploy a Windows 10 VM within that new Resource Group, allowing it to create a new Virtual Network (VNet) . Next, create a Linux (Ubuntu) VM, making sure to select the same Resource Group and the same VNet you just created so that both VMs can communicate on the same private network .
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 2: Observe ICMP Traffic</h2>
Use Remote Desktop to connect to your Windows 10 VM. Inside the VM, install and open Wireshark , start a packet capture, and set the display filter to icmp . Open the command line and ping the private IP address of your Ubuntu VM; observe the ping requests and replies in Wireshark . After that, ping a public website like www.google.com to observe the ICMP traffic leaving your VNet.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 3: Configure Network Security Group (Firewall)</h2>
In the Windows VM's command line, start a continuous ping to the Ubuntu VM (e.g., ping -t <Ubuntu_IP>). Go back to the Azure portal and find the Network Security Group (NSG) associated with your Ubuntu VM. Add a new inbound security rule to deny all ICMP traffic. Switch back to your Windows VM and observe in both the command line and Wireshark that the pings are now failing. To finish, delete the "deny" rule from the NSG ; you will see the ping packets immediately start succeeding again.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 4: Inspect Other Network Protocols</h2>
In Wireshark, change the filter to ssh. From the Windows VM's PowerShell, SSH into your Ubuntu VM (ssh labuser@<private_IP>) . Type a few commands and observe the stream of encrypted SSH traffic in Wireshark. Next, change the Wireshark filter to dhcp. Open an admin PowerShell and run ipconfig /renew to observe the DHCP request/acknowledgment process . After that, filter for dns and use the nslookup google.com command to see the DNS query and response .
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 5: Observe RDP Traffic and Clean Up</h2>
Finally, change the Wireshark filter to tcp.port == 3389 to see RDP traffic. You will observe a non-stop, high-volume spam of traffic; this is because RDP is a constant live-streaming protocol, unlike the other "on-demand" protocols you tested . When finished, close your Remote Desktop connection and delete the entire Resource Group from the Azure portal to remove all created resources and avoid further charges.
</p>
<br />
