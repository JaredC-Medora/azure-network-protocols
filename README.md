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
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
