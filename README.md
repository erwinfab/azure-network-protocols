<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines
*In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.*

### Executive Summary
In this lab, I architected a secure cloud network environment in **Microsoft Azure** consisting of a Windows 10 workstation and an Ubuntu Linux server. I performed deep packet inspection using **Wireshark** to analyze various network protocols (ICMP, SSH, DNS, DHCP, RDP) and implemented firewall rules via **Network Security Groups** to control traffic flow. This project highlights my ability to perform real-time traffic troubleshooting and enforce security policies at the network level.


### Environments and Technologies Used

- **Microsoft Azure** *(Virtual Machines, Virtual Networks, NSGs)*
- **Remote Desktop (RDP)**
- **Various Command-Line Tools** *(Ping, SSH, NSLookup, IPConfig)*
- **Various Network Protocols** *(SSH, RDH, DNS, HTTP/S, ICMP)*
- **Wireshark** *(Protocol Analyzer)*

### Operating Systems Used
- **Windows 10** *(21H2)*
- **Ubuntu Server 20.04 LTS**

## Actions and Observations

**Step 1**: **Create Azure VM Environment**
I created a new **Resource Group** and deployed a Windows 10 VM, allowing it to create a new **Virtual Network (VNet)**. Next, I created an Ubuntu Linux VM, ensuring it was placed in the same Resource Group and VNet so both machines could communicate on the same private network.

---

**Step 2**: **Observe ICMP Traffic**
I used Remote Desktop to connect to the Windows 10 VM via its public IP address. Inside the VM, I installed and opened **Wireshark**, started a packet capture, and set the display filter to **icmp**. I then opened the command line and pinged the private IP address of the Ubuntu VM, observing the live ping requests and replies in Wireshark.

**Step 3**: **Configure Network Security Group (Firewall)**
I started a continuous ping `(ping -t`) from the Windows VM to the Ubuntu VM. In the Azure portal, I located the **Network Security Group (NSG)** for the Ubuntu VM and added an inbound security rule to Deny all ICMP traffic. I observed in both the command line and Wireshark that the pings immediately began to fail with "Request timed out". After deleting the deny rule, the ping packets immediately began succeeding again.

---

**Step 4**: **Inspect Other Network Protocols**

I used Wireshark filters to observe the unique behaviors of several other protocols:

* SSH: Filtered for ssh while logging into the Ubuntu VM from the Windows command line (`ssh labuser@private_ip`) to observe encrypted traffic.

* DHCP: Filtered for dhcp and ran `ipconfig /renew` in an admin PowerShell to observe the DHCP request and acknowledgment process.

* DNS: Filtered for dns and used `nslookup google.com` to see the specific DNS query and response packets.

---

**Step 5**: **Observe RDP Traffic**

Finally, I set the Wireshark filter to **tcp.port == 3389** to inspect **RDP traffic**. I observed a non-stop, high-volume stream of traffic; this is because RDP acts as a constant live-stream between computers, unlike the other "on-demand" protocols tested.













<img width="511" height="354" alt="image" src="https://github.com/user-attachments/assets/58e74b38-0b1e-4d0a-9e51-f84abaf8e617" />
<img width="507" height="315" alt="image" src="https://github.com/user-attachments/assets/2515e4ff-77ea-42d3-9b7a-2cfaa9c9af6e" />
<img width="574" height="37" alt="image" src="https://github.com/user-attachments/assets/6df6487a-0da4-4e49-b44a-a205807fcbab" />
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
<p>
<img width="962" height="463" alt="image" src="https://github.com/user-attachments/assets/1ccc4df8-23fb-45cc-9d22-0a8e2c33ccd6" />
