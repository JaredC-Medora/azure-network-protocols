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

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
<h2>Creating Virtual Machines (VMs)</h2>
Before inspecting traffic, you need to set up two virtual machines in Azure: a Windows 10 VM and a Linux (Ubuntu) VM.


   1. Create a Resource Group:
       - Go to the Azure Portal.
       - In the search bar look up "Resource Group".
       - Click + Add to create a new resource group.
       - Give the resource group a name (e.g., myResourceGroup) and select your desired region.
       - Click Review + Create, then click Create.

<img width="694" alt="Screenshot 2025-03-12 at 12 42 57 PM" src="https://github.com/user-attachments/assets/95f941a0-9782-4e6e-a052-294d58492921" />


   2. Create the Windows 10 Virtual Machine (VM):
       - In the Azure Portal, go to Virtual Machines and click + Add to create a new VM.
       - Subscription: Select your subscription.
       - Resource Group: Select the Resource Group you just created.
       - Virtual Machine Name: Enter a name for your VM, e.g., Windows10-VM.
       - Region: Choose the region for your VM.
       - Image: Select Windows 10 Pro Version 22H2 x64 Gen2.
       - Size: Choose a size with at LEAST 2 vCPUs and 8 GiB.
       - Networking: Select the option to create a new Virtual Network and Subnet. This ensures both VMs are in the same network.
       - Click Review + Create, and then click Create.

   <img width="695" alt="Screenshot 2025-03-11 at 1 36 41 PM" src="https://github.com/user-attachments/assets/d0d68d8f-e940-4173-b0b9-974316cb1455" />

   
   3. Create the Linux (Ubuntu) Virtual Machine (VM):
      - Go to Virtual Machines again, click + Add to create another VM.
      - Subscription: Select your subscription.
      - Resource Group: Select the Resource Group you previously created.
      - Virtual Machine Name: Enter a name for the Linux VM, e.g., Ubuntu-VM.
      - Region: Ensure it is the same region as your Windows VM.
      - Image: Select Ubuntu server 24.04 LTS x64.
      - Size: Choose a suitable size (e.g., Standard B1s).
      - Authentication Type: Choose Password and enter a username (e.g., labuser) and password.
      - Networking: Select the Virtual Network and Subnet created for your Windows VM.
      - Click Review + Create, and then click Create.

![image](https://github.com/user-attachments/assets/6d79a198-ae82-4eec-bfdb-9492c5e055b2)


<p>
</p>
<p>
<h2>Observe ICMP Traffic between VMs</h2>

   1. Install Wireshark on the Windows 10 VM:
      - Once the Windows 10 VM is running, log in using Remote Desktop.
      - Open a web browser on the Windows 10 VM, download and install Wireshark from wireshark.org.

<img width="443" alt="Screenshot 2025-03-11 at 2 12 34 PM" src="https://github.com/user-attachments/assets/a8af287c-0a2b-465a-9058-348d0c5cf45b" />

   2. Start Packet Capture in Wireshark:
      - Open Wireshark on the Windows 10 VM.
      - Start a new packet capture on the appropriate network interface (e.g., Ethernet or Wi-Fi).

<img width="674" alt="Screenshot 2025-03-11 at 2 14 35 PM" src="https://github.com/user-attachments/assets/846d48de-444d-4edc-a42f-a9b33b3f04dd" />


3. Filter for ICMP Traffic:
      - In Wireshark, apply a filter to show only ICMP traffic:
        - In the filter bar, type: icmp and press Enter.

<img width="1297" alt="Screenshot 2025-03-11 at 2 43 01 PM" src="https://github.com/user-attachments/assets/6095593c-dcb0-4fb3-9c3e-6b03b2caf57a" />


4. Ping the Ubuntu VM:
On the Windows 10 VM, open Command Prompt or PowerShell and type:
 ping [Private IP address of Ubuntu VM]
     - Replace [Private IP address of Ubuntu VM] with the actual private IP address of your Ubuntu VM.
     - Observe the ping requests and ping replies in Wireshark.

<img width="799" alt="Screenshot 2025-03-11 at 2 42 46 PM" src="https://github.com/user-attachments/assets/3d3ab234-4d2d-4b91-8ed4-c0797c53b266" />

</p>
<br />

<p>
</p>
<h2>Configure Firewall (Network Security Group) and Inspect More Traffic</h2>

1. Observe and Control ICMP Traffic:
Initiate a Perpetual Ping from the Windows 10 VM to the Ubuntu VM by running the following command in PowerShell:
 ping [Ubuntu VM IP address] -t

<img width="296" alt="Screenshot 2025-03-11 at 2 46 30 PM" src="https://github.com/user-attachments/assets/ab3e447a-56da-47d3-b982-c92bd91382c3" />


 - Disable ICMP Traffic:
   - In the Azure portal, go to Network Security Groups.
   - Find the NSG associated with the Ubuntu VM.
   - Under Inbound security rules, add a rule to Deny ICMP (type ICMP and set the action to Deny).

 <img width="767" alt="Screenshot 2025-03-11 at 2 45 07 PM" src="https://github.com/user-attachments/assets/f09dacc9-75e2-475e-aeaf-796bcf06f6c0" />

<img width="1293" alt="Screenshot 2025-03-11 at 2 45 50 PM" src="https://github.com/user-attachments/assets/808b9803-451c-4214-949d-105fed8c3149" />

 - Observe in Wireshark/Powershell:
   - Return to Wireshark/Powershell and observe the lack of ICMP traffic in the capture.
   - The perpetual ping should stop.

<img width="770" alt="Screenshot 2025-03-11 at 2 46 51 PM" src="https://github.com/user-attachments/assets/e1d93c11-4605-43b4-95d5-3d711d4402a4" />

 - Re-enable ICMP Traffic:
   - Go back to the Network Security Group and remove or disable the Deny ICMP rule.
 - Observe the Traffic Again:
   - In Wireshark/Powershell, observe the ICMP traffic return, and the perpetual ping should resume.

<img width="788" alt="Screenshot 2025-03-11 at 2 47 57 PM" src="https://github.com/user-attachments/assets/5646cec8-44f5-4a9a-9f22-fea73c7a959c" />

2. Observe SSH Traffic:
 - Start a Packet Capture in Wireshark and filter for SSH traffic:
   - Use the filter: tcp.port == 22.

<img width="126" alt="Screenshot 2025-03-11 at 2 51 04 PM" src="https://github.com/user-attachments/assets/c8d34460-7e14-4a2a-9a81-d3118011fd45" />

 - SSH into the Ubuntu VM from the Windows 10 VM:
Open PowerShell on the Windows VM and run:
 ssh labuser@[Private IP address of Ubuntu VM]

<img width="544" alt="Screenshot 2025-03-12 at 1 04 34 PM" src="https://github.com/user-attachments/assets/8a064ec0-41e5-49ea-a3a0-48a08c1a3cca" />

   - Type the password when prompted.
 - Observe the SSH Traffic in Wireshark as you interact with the Ubuntu VM.

<img width="1287" alt="Screenshot 2025-03-12 at 1 08 47 PM" src="https://github.com/user-attachments/assets/52b6221d-3acf-458a-8558-f521caf91890" />
 
 - Exit the SSH session by typing exit and pressing Enter.

<img width="171" alt="Screenshot 2025-03-11 at 2 51 51 PM" src="https://github.com/user-attachments/assets/ba51c08d-10b5-44ce-b906-e1ecbee6b89a" />

3. Observe DHCP Traffic:
 - Start a Packet Capture in Wireshark and filter for DHCP traffic:
   - Use the filter: dhcp.

 <img width="1296" alt="Screenshot 2025-03-12 at 1 11 10 PM" src="https://github.com/user-attachments/assets/341c7d8d-521a-4358-8cad-c59bb2185bc4" />

 - Renew the IP Address of the Windows 10 VM:
Open Notepad and type the following:
 ipconfig /renew
 ipconfig /release
 - Save the file to the C: drive in the "Program Data" Folder

<img width="259" alt="Screenshot 2025-03-11 at 2 53 09 PM" src="https://github.com/user-attachments/assets/7be143e0-2341-4505-a873-fafbadbc6ae5" />

 - Name the file dhcp.bat

 <img width="148" alt="Screenshot 2025-03-11 at 2 53 20 PM" src="https://github.com/user-attachments/assets/d9326849-6131-48c8-8fd8-13c6a25ea05b" />

 - In Powershell execute the following:

<img width="774" alt="Screenshot 2025-03-12 at 1 17 45 PM" src="https://github.com/user-attachments/assets/2275d1fc-7c4d-48cf-947c-9125a84f2c5e" />

 - Observe the DHCP traffic in Wireshark, which will show the DHCP request and response.

<img width="1296" alt="Screenshot 2025-03-12 at 1 19 23 PM" src="https://github.com/user-attachments/assets/a4f81c4d-8a1d-4134-a7f4-6283ae822e9f" />

4. Observe DNS Traffic:
 - Start a Packet Capture in Wireshark and filter for DNS traffic:
   - Use the filter: dns.

 <img width="1295" alt="Screenshot 2025-03-12 at 1 20 27 PM" src="https://github.com/user-attachments/assets/882cf1c2-8ddf-465f-bba1-f93edaa3eaff" />

 - Use nslookup to query for the IP addresses of websites like google.com and disney.com:
In PowerShell or Command Prompt:
 nslookup google.com
nslookup disney.com

<img width="317" alt="Screenshot 2025-03-11 at 2 54 34 PM" src="https://github.com/user-attachments/assets/ab47552b-2543-456d-927d-f130ff2aa7bf" />

 - Observe the DNS traffic in Wireshark as DNS queries are sent and responses are received.

<img width="1294" alt="Screenshot 2025-03-12 at 1 22 36 PM" src="https://github.com/user-attachments/assets/7c3fb8ec-ae4c-4f2c-8c56-7c421772c424" />

5. Observe RDP Traffic:
 - Start a Packet Capture in Wireshark and filter for RDP traffic:
   - Use the filter: tcp.port == 3389.

 <img width="1279" alt="Screenshot 2025-03-12 at 1 23 55 PM" src="https://github.com/user-attachments/assets/94f96718-db23-4e3a-85e2-5662c8205ea8" />

 - Observe the constant stream of RDP traffic:
   - RDP (Remote Desktop Protocol) continuously sends traffic for the live stream of a remote desktop session.
   - Observe the traffic in Wireshark. It's constant because RDP keeps sending updates for the live session.

</p>
<br />

<p>
</p>
<p>
<h2>PLEASE DO NOT FORGET TO DELETE/PAUSE ALL VMs in Azure to Reduce ANY costs or charges</h2>
</p>
<br />
