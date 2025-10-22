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

- Step 1: Create an Azure environment with a Windows 10 and an Ubuntu VM in the same Virtual Network .
- Step 2: Use Wireshark to capture and observe ICMP (ping) traffic between the two VMs and to the public internet .
- Step 3: Configure the Network Security Group (NSG) to create a firewall rule that blocks ICMP traffic, and then re-enable it .
- Step 4: Inspect various network protocols (SSH, DHCP, DNS, and RDP) in Wireshark to observe their unique behaviors .

<h2>Actions and Observations</h2>

<p>

<img width="548" height="170" alt="image" src="https://github.com/user-attachments/assets/b7e2593d-b544-4c4d-9693-6cd3e827c887" />
<h2></h2>
<img width="469" height="325" alt="image" src="https://github.com/user-attachments/assets/e263214a-8e4e-424d-8643-6cca5e4309b5" /> <img width="464" height="198" alt="image" src="https://github.com/user-attachments/assets/4a2e7a67-b2ca-4e50-9906-1b69209e4a31" />
<img width="462" height="32" alt="image" src="https://github.com/user-attachments/assets/704182cc-0767-4445-a86a-f7e04b834f74" />
<h2></h2>
<img width="465" height="291" alt="image" src="https://github.com/user-attachments/assets/c25366be-88c7-4728-80b1-f43cda65c8d9" /> <img width="464" height="140" alt="image" src="https://github.com/user-attachments/assets/51ae8992-e4d3-4e99-a88c-1b726ac7833a" />

<img width="463" height="31" alt="image" src="https://github.com/user-attachments/assets/290c21a0-c2ab-4b4e-a052-4189fc72cc7c" /> 




</p>
<p>
<h2>Step 1: Create Azure VM Environment</h2>
First, in the Azure portal, create a new Resource Group. Deploy a Windows 10 VM within that new Resource Group, allowing it to create a new Virtual Network (VNet) . Next, create a Linux (Ubuntu) VM, making sure to select the same Resource Group and the same VNet you just created so that both VMs can communicate on the same private network .
</p>
<br />

<p>
<img width="302" height="233" alt="image" src="https://github.com/user-attachments/assets/56a7f88b-0345-4769-89b4-cfc9323213ad" />
<h2></h2>
<img width="316" height="178" alt="image" src="https://github.com/user-attachments/assets/ece01822-5a2e-430d-824d-41502f4226c3" /> <img width="429" height="95" alt="image" src="https://github.com/user-attachments/assets/8cdc5ac7-4691-42ba-8e4e-0a95a69d6e3d" />
<h2></h2>
<img width="492" height="336" alt="image" src="https://github.com/user-attachments/assets/768ca6f5-f61a-4067-9e9c-73cbb6bce9b0" /><img width="836" height="221" alt="image" src="https://github.com/user-attachments/assets/546a698d-d14e-4dbf-9e6f-b526e1678036" />
<h2></h2>



</p>
<p>
<h2>Step 2: Observe ICMP Traffic</h2>
Use Remote Desktop to connect to your Windows 10 VM (Public IP-Address). Inside the VM, install and open Wireshark (Protocol Analyzer) , start a packet capture, and set the display filter to icmp . Open the command line and ping the private IP address(Ex. 10.0.0.5) of your Ubuntu VM; observe the ping requests and replies in Wireshark . 
</p>
<br />

<p>
<img width="460" height="452" alt="image" src="https://github.com/user-attachments/assets/45ae3c58-961d-4002-a96d-fa215244f77a" />
<h2></h2>
<img width="390" height="87" alt="image" src="https://github.com/user-attachments/assets/0d123d95-17b0-4464-aa29-bdd5a446ccd4" />
<img width="713" height="46" alt="image" src="https://github.com/user-attachments/assets/bd7c92b2-d60d-49d6-84ee-6b964fc2ec55" />
<h2></h2>
<img width="364" height="154" alt="image" src="https://github.com/user-attachments/assets/09b0bb61-5a56-4e67-99fb-8108222c0152" /> <img width="411" height="153" alt="image" src="https://github.com/user-attachments/assets/dfa06b02-da53-4a81-8c41-174fa8788aaa" />
<h2></h2>
<img width="310" height="70" alt="image" src="https://github.com/user-attachments/assets/f4fa5a06-76f5-4244-bc51-6802779f36d3" /> <img width="364" height="177" alt="image" src="https://github.com/user-attachments/assets/24fbf311-f0cb-49f5-973d-a65b52827a23" /> <img width="387" height="102" alt="image" src="https://github.com/user-attachments/assets/f3e4d5b1-cd74-4014-b4bb-0246f383d1a0" />




</p>
<p>
<h2>Step 3: Configure Network Security Group (Firewall)</h2>
In the Windows VM's command line, start a continuous ping to the Ubuntu VM (e.g., ping -t <Ubuntu_IP>). Go back to the Azure portal and find the Network Security Group (NSG) associated with your Ubuntu VM. Add a new inbound security rule to deny all ICMP traffic. Switch back to your Windows VM and observe in both the command line and Wireshark that the pings are now failing. To finish, delete the "deny" rule from the NSG ; you will see the ping packets immediately start succeeding again.
</p>
<br />

<p>
<img width="420" height="95" alt="image" src="https://github.com/user-attachments/assets/069f1d96-8bce-4088-8c07-ba0c64673406" /> <img width="621" height="119" alt="image" src="https://github.com/user-attachments/assets/9a3c63ed-442e-43f4-93c8-b3e63ebca88e" /> <img width="509" height="196" alt="image" src="https://github.com/user-attachments/assets/7bd6a7f7-159b-43e6-bc3b-e5bf22f8d36f" />
<img width="604" height="261" alt="image" src="https://github.com/user-attachments/assets/f2421b2e-9a15-4473-85ed-3563fc193d48" />
<h2></h2>
<img width="421" height="131" alt="image" src="https://github.com/user-attachments/assets/fc049868-7386-4025-b5f3-e499b60be223" /> <img width="220" height="60" alt="image" src="https://github.com/user-attachments/assets/b6077093-f217-48d2-a58e-5cf8214907e5" />
<h2></h2>
<img width="521" height="368" alt="image" src="https://github.com/user-attachments/assets/5736ddf3-eaf6-447a-bff8-577fa02a87af" />
<img width="753" height="166" alt="image" src="https://github.com/user-attachments/assets/6f954ace-1051-4159-a8a4-2ad264c906ba" />
<h2></h2>

<img width="335" height="99" alt="image" src="https://github.com/user-attachments/assets/95446f6e-6251-41a5-92e3-55c27b5f5d0d" /> <img width="316" height="164" alt="image" src="https://github.com/user-attachments/assets/af38c733-dc8a-4a7a-95b7-4906f6163e1c" />
<img width="967" height="481" alt="image" src="https://github.com/user-attachments/assets/6c790ae9-571a-4925-b882-cb33af738d3b" />


</p>
<p>
<h2>Step 4: Inspect Other Network Protocols</h2>
In Wireshark, change the filter to ssh. From the Windows VM's PowerShell, SSH into your Ubuntu VM (ssh labuser@<private_IP>) . Type a few commands and observe the stream of encrypted SSH traffic in Wireshark. Next, change the Wireshark filter to dhcp. Open an admin PowerShell and run ipconfig /renew to observe the DHCP request/acknowledgment process . After that, filter for dns and use the nslookup disney.com command to see the DNS query and response .
</p>
<br />

<p>
<img width="962" height="463" alt="image" src="https://github.com/user-attachments/assets/1ccc4df8-23fb-45cc-9d22-0a8e2c33ccd6" />

</p>
<p>
<h2>Step 5: Observe RDP Traffic</h2>
Finally, change the Wireshark filter to tcp.port == 3389 to see RDP traffic. You will observe a non-stop, high-volume spam of traffic; this is because RDP is a constant live-streaming protocol, unlike the other "on-demand" protocols you tested .
</p>
<br />
