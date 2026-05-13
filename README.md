<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines
*In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.*

### Executive Summary
In this lab, I architected a secure cloud network environment in **Microsoft Azure** consisting of a Windows 10 workstation and an Ubuntu Linux server. I performed deep packet inspection using **Wireshark** to analyze various network protocols (ICMP, SSH, DNS, DHCP, RDP) and implemented firewall rules via **Network Security Groups** to control traffic flow. This project highlights my ability to perform real-time traffic troubleshooting and enforce security policies at the network level.


### Environments and Technologies Used

- **Microsoft Azure** *(Virtual Machines, Virtual Networks, NSGs)*
- **Remote Desktop Protocol (RDP)**
- **Various Command-Line Tools** *(Ping, SSH, NSLookup, IPConfig)*
- **Various Network Protocols** *(SSH, RDH, DNS, HTTP/S, ICMP)*
- **Wireshark** *(Protocol Analyzer)*

### Operating Systems Used
- **Windows 10** *(21H2)*
- **Ubuntu Server 20.04 LTS**

## Actions and Observations

**Step 1**: **Create Azure VM Environment**

I created a new **Resource Group** and deployed a Windows 10 VM, allowing it to create a new **Virtual Network (VNet)**. Next, I created an Ubuntu Linux VM, ensuring it was placed in the same Resource Group and VNet so both machines could communicate on the same private network.

* Resource group created.
  
<img width="1592" height="319" alt="image" src="https://github.com/user-attachments/assets/0a8094be-8a95-4c71-af9f-e37427fce822" />


* Created a VNet.
  
<img width="1569" height="845" alt="image" src="https://github.com/user-attachments/assets/20e4f3fd-744d-4dcb-918e-64502dd03749" />


* Windows and Linux VM in the same Resource Group.
  
<img width="1592" height="355" alt="image" src="https://github.com/user-attachments/assets/4cfec81a-af7e-4c99-8d47-bad00a06184f" />

---

**Step 2**: **Observe ICMP Traffic**

I used Remote Desktop to connect to the Windows 10 VM via its public IP address. Inside the VM, I installed and opened **Wireshark**, started a packet capture, and set the display filter to **icmp**. I then opened the command line and pinged the private IP address of the Ubuntu VM, observing the live ping requests and replies in Wireshark.

* Wireshark installation on Windows 10 VM.
  
<img width="1001" height="814" alt="image" src="https://github.com/user-attachments/assets/20afaa47-3eed-4bfb-bfc8-0c085eaa8e2e" />

* Set ICMP display filter and pinged Ubuntu VM private IP address.
  
<img width="1601" height="881" alt="image" src="https://github.com/user-attachments/assets/9fb8d012-8bf5-470e-8cc0-26a448c3a22b" />

---

**Step 3**: **Configure Network Security Group (Firewall)**

I started a continuous ping (`ping -t`) from the Windows VM to the Ubuntu VM. In the Azure portal, I located the **Network Security Group (NSG)** for the Ubuntu VM and added an inbound security rule to Deny all ICMP traffic. I observed in both the command line and Wireshark that the pings immediately began to fail with "Request timed out". After deleting the deny rule, the ping packets immediately began succeeding again.

* Used command "ping -t" for a continous Ubuntu VM ping.

<img width="1596" height="880" alt="image" src="https://github.com/user-attachments/assets/d78f7509-68fa-4d71-8fd6-a0b37f63cc61" />

* Added an inbound security rule to Deny all ICMP traffic.

<img width="1438" height="522" alt="image" src="https://github.com/user-attachments/assets/11571774-a04e-483f-95c5-421d5b64e9c7" />

* Pings "Request timed out" after inbound security rule.

<img width="1451" height="807" alt="image" src="https://github.com/user-attachments/assets/c8beede4-1fd1-436e-b0fe-f075d52e0274" />

---

**Step 4**: **Inspect Other Network Protocols**

I used Wireshark filters to observe the unique behaviors of several other protocols:

* **SSH**: Filtered for ssh while logging into the Ubuntu VM from the Windows command line (`ssh labuser @private_ip`) to observe encrypted traffic.

<img width="1455" height="840" alt="image" src="https://github.com/user-attachments/assets/5aeb6a51-a96e-4aa5-a04a-e5a62c79ffbb" />


* **DHCP**: Filtered for dhcp and ran `ipconfig /renew` in an admin PowerShell to observe the DHCP request and acknowledgment process.

<img width="1440" height="814" alt="image" src="https://github.com/user-attachments/assets/bae7761d-8a0b-4aec-a9a1-db3cdacac69b" />


* **DNS**: Filtered for dns and used `nslookup google.com` to see the specific DNS query and response packets.

<img width="1453" height="849" alt="image" src="https://github.com/user-attachments/assets/8f2405bc-cdb1-4a31-a991-c8b7b58da9fb" />

---

**Step 5**: **Observe RDP Traffic**

Finally, I set the Wireshark filter to **tcp.port == 3389** to inspect **RDP traffic**. I observed a non-stop, high-volume stream of traffic; this is because RDP acts as a constant live-stream between computers, unlike the other "on-demand" protocols tested.

<img width="1453" height="849" alt="image" src="https://github.com/user-attachments/assets/207d1a8d-a3d5-4635-8615-99d8e3423f57" />

---

### Technical Skills Highlighted

**Network Security**: Implementation of stateful firewall rules via Azure NSGs.

**Traffic Analysis**: Real-time packet capture and protocol identification using Wireshark.

**Protocol Knowledge**: Deep understanding of the OSI model, specifically Layers 3, 4, and 7.

**Cloud Infrastructure**: Managing interconnected resources within an Azure Virtual Network.



