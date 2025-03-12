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
Steps:
   1. Create a Resource Group:
       - Go to the Azure Portal.
       - In the left-hand menu, click on Resource groups.
       - Click + Add to create a new resource group.
       - Give the resource group a name (e.g., myResourceGroup) and select your desired region.
       - Click Review + Create, then click Create.

   2. Create the Windows 10 Virtual Machine (VM):
       - In the Azure Portal, go to Virtual Machines and click + Add to create a new VM.
       - Subscription: Select your subscription.
       - Resource Group: Select the Resource Group you just created.
       - Virtual Machine Name: Enter a name for your VM, e.g., Windows10-VM.
       - Region: Choose the region for your VM.
       - Image: Select Windows 10.
       - Size: Choose the size based on your needs (e.g., Standard B1s).
       - Authentication Type: Choose Password, then enter a username and password (e.g., admin and a secure password).
       - Networking: Select the option to create a new Virtual Network and Subnet. This ensures both VMs are in the same network.
       - Click Review + Create, and then click Create.
   
   
   3. Create the Linux (Ubuntu) Virtual Machine (VM):
      - Go to Virtual Machines again, click + Add to create another VM.
      - Subscription: Select your subscription.
      - Resource Group: Select the Resource Group you previously created.
      - Virtual Machine Name: Enter a name for the Linux VM, e.g., Ubuntu-VM.
      - Region: Ensure it is the same region as your Windows VM.
      - Image: Select Ubuntu.
      - Size: Choose a suitable size (e.g., Standard B1s).
      - Authentication Type: Choose Password and enter a username (e.g., labuser) and password.
      - Networking: Select the Virtual Network and Subnet created for your Windows VM.
      - Click Review + Create, and then click Create.
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Observe ICMP Traffic between VMs</h2>

   1. Install Wireshark on the Windows 10 VM:
      - Once the Windows 10 VM is running, log in using Remote Desktop.
      - Open a web browser on the Windows 10 VM, download and install Wireshark from wireshark.org.

   2. Start Packet Capture in Wireshark:
      - Open Wireshark on the Windows 10 VM.
      - Start a new packet capture on the appropriate network interface (e.g., Ethernet or Wi-Fi).

3. Filter for ICMP Traffic:
      - In Wireshark, apply a filter to show only ICMP traffic:
        - In the filter bar, type: icmp and press Enter.

4. Ping the Ubuntu VM:
On the Windows 10 VM, open Command Prompt or PowerShell and type:
 ping [Private IP address of Ubuntu VM]
     - Replace [Private IP address of Ubuntu VM] with the actual private IP address of your Ubuntu VM.
     - Observe the ping requests and ping replies in Wireshark.

5. Ping a Public Website:
From the Windows 10 VM, open Command Prompt or PowerShell and type:
 ping google.com
     - Observe the ICMP traffic from the public website in Wireshark.
</p>
<br />

<p>
<img src="https://imgur.com/a/YL6Si9s" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>Configure Firewall (Network Security Group) and Inspect More Traffic</h2>

1. Observe and Control ICMP Traffic:
Initiate a Perpetual Ping from the Windows 10 VM to the Ubuntu VM by running the following command in PowerShell:
 ping [Ubuntu VM IP address] -t


 - Disable ICMP Traffic:
   - In the Azure portal, go to Network Security Groups.
   - Find the NSG associated with the Ubuntu VM.
   - Under Inbound security rules, add a rule to Deny ICMP (type ICMP and set the action to Deny).
 - Observe in Wireshark:
   - Return to Wireshark and observe the lack of ICMP traffic in the capture.
   - The perpetual ping should stop.
 - Re-enable ICMP Traffic:
   - Go back to the Network Security Group and remove or disable the Deny ICMP rule.
 - Observe the Traffic Again:
   - In Wireshark, observe the ICMP traffic return, and the perpetual ping should resume.

2. Observe SSH Traffic:
 - Start a Packet Capture in Wireshark and filter for SSH traffic:
   - Use the filter: tcp.port == 22.
 - SSH into the Ubuntu VM from the Windows 10 VM:
Open PowerShell on the Windows VM and run:
 ssh labuser@[Private IP address of Ubuntu VM]

   - Type the password when prompted.
 - Observe the SSH Traffic in Wireshark as you interact with the Ubuntu VM.
 - Exit the SSH session by typing exit and pressing Enter.

3. Observe DHCP Traffic:
 - Start a Packet Capture in Wireshark and filter for DHCP traffic:
   - Use the filter: dhcp.
 - Renew the IP Address of the Windows 10 VM:
Open PowerShell as an Administrator and run:
 ipconfig /renew

 - Observe the DHCP traffic in Wireshark, which will show the DHCP request and response.

4. Observe DNS Traffic:
 - Start a Packet Capture in Wireshark and filter for DNS traffic:
   - Use the filter: dns.
 - Use nslookup to query for the IP addresses of websites like google.com and disney.com:
In PowerShell or Command Prompt:
 nslookup google.com
nslookup disney.com

 - Observe the DNS traffic in Wireshark as DNS queries are sent and responses are received.

5. Observe RDP Traffic:
 - Start a Packet Capture in Wireshark and filter for RDP traffic:
   - Use the filter: tcp.port == 3389.
 - Observe the constant stream of RDP traffic:
   - RDP (Remote Desktop Protocol) continuously sends traffic for the live stream of a remote desktop session.
   - Observe the traffic in Wireshark. It's constant because RDP keeps sending updates for the live session.

</p>
<br />

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
