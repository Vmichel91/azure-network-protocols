<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)
- PowerShell (Command-Line Interface)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Create Virtual Machines
- Step 2: Remote Desktop into VMs
- Step 3: Install & Run WireShark
- Step 4: Test Connectivity Between VMs
- Step 5: Alter Network Security Group Settings
- Step 6: SSH into VM
- Step 7: Observe DHCP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://imgur.com/0INwRXw.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/tc32g9Q.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 1: Set up two Virtual Machines in Azure, begin by logging in to the Microsoft Azure Portal with your Microsoft account. Next, search for "resource groups" and create a new one. Then, search for "Virtual Machines" and create two separate VMs, one with Windows 10 OS and the other with Ubuntu OS. Ensure that both VMs are placed in the same resource group. Keep in mind to remember the username and password used for the setup process, as it will be necessary for remote desktop access later. It is recommended to use the same login information for both VMs.
</p>
<br />

<p>
<img src="https://imgur.com/Dhd0N4w.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/c8UPweu.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2: Remotely access each Virtual Machine by following these steps:

-Go to the settings of VM1 and copy its "public IP address"
-Open the "Remote Desktop Connection" application by searching for it in the start menu search bar
-Paste the copied IP address from VM1 into the Remote Desktop Connection and click connect
-Use the username and password created in step 1 to log in to VM1. Repeat the same process to log into VM2.
</p>
<br />

<p>
<img src="https://imgur.com/uDS8XIY.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/fv4AE4n.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/Bxdbca7.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3: Next download WireShark in VM1, follow these steps:

-Search for "WireShark" on Google and click on the official website
-Download the "Windows Installer (64-bit)" version
-Follow the prompts to install WireShark
-Once installed, open WireShark and select "Ethernet 2"
-Click the shark symbol in the upper left corner to start monitoring network traffic
-Use the display filter bar to filter out irrelevant network traffic by typing "icmp" in the filter bar to only see ping traffic.
</p>
<br />

<p>
<img src="https://imgur.com/6cvo6nr.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/vBozpzR.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/EJCQRcw.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: Time to observe communication between the two VMs:

-Go to VM2 on the Azure Portal and copy its "private IP address" under the "overview" tab
-On VM1, search for "PowerShell" in the start menu and open it
-Type "ping -t" followed by pasting VM2's private IP address into PowerShell to initiate a "perpetual ping"
-Observe the communication between the two VMs through WireShark, which will show the VMs pinging back and forth. Allow the ping to continue and pay attention to the data being communicated between both VMs.
</p>
<br />

<p>
<img src="https://imgur.com/5xWsHFk.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/AgVXdZo.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In step 5, we will see what happens when we block ICMP traffic to VM2 using the firewall settings:

-Go back to the Azure Portal and search for "Network Security Group"
-Select "VM2-NSG"
-Under settings, navigate to "Inbound security rules"
-Click "+ add" and in the new window change "protocol" to "ICMP", "action" to "deny" and "Name" to "Deny_ICMP_Ping"
-Click "add" at the bottom.
-*Notice that the perpetual ping command on VM1, initiated in step 4, will stop due to being blocked by VM2's firewall as a result of the new inbound security rule you created. If you edit the rule to allow traffic, the ping will resume. To stop the PowerShell ping, press Ctrl+C"
</p>
<br />

<p>
<img src="https://imgur.com/Bf1IhYe.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/15ct7pU.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
   Step 6:
To access VM2 from VM1 via SSH, follow these steps:

In WireShark, filter for "SSH" (referenced in step 3)
In PowerShell, type "ssh labuser@(VM1 private IP address)" into the command line (labuser was what i used when setting up VM, please use your username that you created)
Type "yes" to the connection prompt, then enter the password (even though it may not appear on the screen)
You have now successfully logged into VM2's command-line interface (CLI) and it should display "labuser@VM2:"
You can type Linux commands such as "id" to see new network traffic between the VMs since connecting via SSH. When finished exploring, type "exit" in the command line in PowerShell to end the connection.
</p>
<br />

<p>
<img src="https://imgur.com/UdMf24u.gif" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In step 7, observe DHCP traffic in the same way as with SSH:

In WireShark, filter for "DHCP" (referenced in step 3)
In PowerShell, type "ipconfig /renew" into the command line to request a new IP address for VM1 from the Azure DHCP server. Note that this may temporarily disconnect the VM, if so, reconnect through Remote Desktop Connection.
Observe the newly issued IP address in WireShark. This concludes the lab."
</p>
<br />
