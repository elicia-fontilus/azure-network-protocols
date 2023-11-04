<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Windows Powershell
- Various Network Protocols (DHCP, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Prerequisites</h2>

  -  [Create Virtual Machines within Azure](https://github.com/elicia-fontilus/Creating-Virtual-Machines-in-Azure)


<h2>High-Level Steps</h2>

- Step 1: Create a Windows 10 Virtual Machine (VM) & Create a Linux (Ubuntu) VM
- Step 2: Use Remote Desktop to connect to your Windows 10 VM
- Step 3: Within your Windows 10 Virtual Machine, Install Wireshark
- Step 4: Open Wireshark and filter for ICMP traffic only
- Step 5: Ping Ubuntu VM from within the Windows 10 VM 
- Step 6: Open the Network Security Group your Ubuntu VM is using, disable and re-enable incoming ICMP traffic
- Step 7: Observe DHCP Traffic
- Step 8: Observe DNS Traffic
- Step 9: Observe RDP Traffic
- Step 10: Cleanup

<h2>Actions and Observations</h2>

<p>

![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/965d5560-69b0-4812-aadf-86a3cab3f20d)
</p>
<p>
First you'll want to go to https://portal.azure.com
Create 2 VM's one running Windows 10 and the other running on Ubuntu. Then get the public address of Windows 10 VM, which can be found in the virtual machine section.    </p>
<br />

<p>
 
  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/98bd909e-33be-46e9-883c-2fac7f56252b)
Paste Windows 10 public IP address into Remote Desktop Connection 
</p>
<p></p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/c7d07827-b489-418e-a7d0-c74950fd1d2e)
  Enter the username and password that we made while setting up the VM
<p>
</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/7d841398-873d-4b39-8215-f2d7a515311d)
</p>
<p>
Open your internet brower of choice search "wireshark download". Click the first link. Next, you'll download the "Windows x64 Installer" version of wireshark which should be the first link on the page. Once the download is completed you will want to open file (the top right) and finish the rest of Wireshark download. </p>
<br />

<p>

![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/df59f68f-7ebc-456b-97f8-3905386131a2)
</p>
Download Wireshark with it's default settings </p>
<br />

<p>

</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/d0107751-b87b-4fb0-9c9f-7ca17b36fa3d)
</p>
<p>
Open Wireshark and filter the traffic by ICMP</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/6664d000-687e-4d97-a1c2-016c672c7638)
</p>
<p>
In the Azure portal Ubuntu private IP address can be in the VM's overview under the networking section. Copy IP address  </p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/dd3fac37-528a-4ffc-bb10-580a80615708)
</p>
<p>
Now open Powershell by clicking start in Windows 10 VM and type Powershell. 
 Next to initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM we must type in "ping (Ubuntu priavte IP address) -t"</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/8c66c6d6-fd8e-4eff-9426-cf665f7f4d40)
</p>
Observe ping requests and replies within WireShark. We will leave this to ping non-stop while we go back to the Azure portal and change this VM's firewall to block incoming ICMP traffic. <p>
</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/3dcf872d-fa17-473c-9179-fa1de3e1208b)
</p>
<p>
Back in Azure we will type in Network, select Network Security Group (NSG), select your Ubuntu VM , then inbound security rules, and click "+ add"  </p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/03855b98-c942-40b2-a152-4715ebe2bd7e)
</p>
<p>
Select Protocol: ICMP, Action:Deny, Priority:200 (because we want the rule followed above all the others), type any name, and click add. 
Refresh the page. and now back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
 </p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/68983698-420c-445d-bb0a-9953acbc02f7)
</p>
<p>
Just requests being sent out no replies because we disabled ICMP traffic. In order to enable ICMP traffic we must go back to our NSG and allow the traffic.</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/5f38ea2d-41c0-45c6-903a-7786feace910)
</p>
<p>
In NSG allow ICMP traffic by simply selecting action:allow. Refresh </p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/460f8f03-f538-48a6-8718-719470a98682)
</p>
<p>
Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity will start working
</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/368601a3-b3f5-4820-b3bb-f6aa140efde6)
</p>
<p>
Stop the ping activity by "ctrl+c"
</p>
<br />

<p>

![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/fd1c1df2-dacd-4c7b-be72-d9cdd289f94f)
</p>
<p>
Observe DHCP Traffic

From your Windows 10 VM, issue your VM a new IP address from the command line by typing ipconfig /renew

</p>
<br />

<p>

![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/83993a3c-ab88-420e-9389-57bd97051ab7)
</p>
<p>
Observe DNS Traffic

From your Windows 10 VM within command line, use nslookup to see what google.com and disney.com’s IP addresses are
</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/eb23839b-3ad5-453d-bf3a-698c6c39d608)
</p>
<p>
Observe RDP Traffic

Filter for RDP traffic only tcp.port == 3389, which spammed with traffic because RDP is constantly at work between the 2 computers so therefore traffic will always be flowing.</p>
<br />

<p>

  ![image](https://github.com/elicia-fontilus/azure-network-protocols/assets/149262013/df254ca4-3902-49a9-9477-992c7709ae58)
</p>
<p>
Exit Remote Connection and Delete Resource Group so you don't incur costs </p>
<br />
