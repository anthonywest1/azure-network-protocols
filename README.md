<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create our Virtual Machines
- Configuring a Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic

  

<h2>Actions and Observations</h2>

<p>
<img width="1435" alt="PORTFOLIO CREATE VM STEP 1 NSG" src="https://github.com/user-attachments/assets/784a9464-85f1-40e4-8c2f-be56d6f29e21" />
</p>
<p>
Begin with setting up Ip addresses from Azure into the VMs -> Assuming VMs were turned off from the day before, tick the VMs in Azure 
<br />  
  
<p>
<img width="637" alt="create resource group NSG STEP 1" src="https://github.com/user-attachments/assets/dbfcfd47-e2b3-4d97-a200-be0dbed71d0a"/>
<img width="880" alt="NSG CREATE WINDOWS 10 PRO STEP 2" src="https://github.com/user-attachments/assets/7d660bf7-06ed-4d3e-9263-a58f5fe622c6"/>
</p>
<p>
Press Start to begin now working in VMs -> Create a Resource Group -> Create a Windows 10 Virtual Machine (VM) using the previously created Resource Group -> adjust settings correctly -> create -> While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet
</p>
<br />  

<p>
<img width="1440" alt="nsg STEP 2 UBUNTU" src="https://github.com/user-attachments/assets/2f53c180-fe4c-409f-b87c-f4a6ab3f1e44"/>
<img width="1119" alt="ENSURE BOTH IN SAME VNET STEP 1" src="https://github.com/user-attachments/assets/9f1cb112-84ab-4493-b7c9-5a6111ca47db"/>
<img width="1113" alt="ENSURE BOTH IN SAME VNET STEP 1 SAME" src="https://github.com/user-attachments/assets/38f0aa75-e52a-4bba-9014-2cc5030b7aac"/>
</p>
<p> Now, Create a Linux (Ubuntu) VM(While creating the VM, select the previously created Resource Group and Virtual Network—the Virtual Network MUST BE THE SAME) -> Authentication type: Username/Password NOT SSH public key -> Next -> Next -> Ensure both VMs are in the same Virtual Network / Subnet 
</p>
<br />

<p>
<img width="1440" alt="STEP 2 - GET IP ADD NSG" src="https://github.com/user-attachments/assets/6a10de0f-aa5b-414e-9fc5-ba6d86cd146b"/>
<img width="1156" alt="ADD into VM NSG STEP 2" src="https://github.com/user-attachments/assets/fb7ab0c2-7961-4404-b972-3d862e15217c"/>
<img width="1440" alt="STEP 2 OPEN WIRESHARK START CAPTURE" src="https://github.com/user-attachments/assets/a02fd3b0-37e9-42ef-b103-fcc8754e86d6"/>
</p>
<p> If using Mac, install Microsoft Remote Desktop -> Use Remote Desktop to connect to Windows 10 Virtual Machine by getting the Ip address to put into the Microsoft VM, Install Wireshark -> Open Wireshark and start packet capture 
  

<p>
<img width="1440" alt="icmp filtered only step 2 observe request and reply in WS" src="https://github.com/user-attachments/assets/b82e1752-bb95-4334-b5a4-9a7c378b4315"/>
<img width="1440" alt="STEP 3 nonstop ping from windows to ubun" src="https://github.com/user-attachments/assets/b6044d67-61bd-438b-976b-3c6b5d1d569e"/>
</p>
<p>
Now, Within Wireshark, filter for ICMP traffic only -> Now we Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM -> Open PSH from start menu 
</p>
<br />


<p>
<img width="1436" alt="Ping private ip of Ubuntu to observe ping request " src="https://github.com/user-attachments/assets/d9af1d0f-5204-4d04-a691-d4ecaa5a0909"/>
</p>
<p>
Now From Windows VM I will attempt to ping Linux VM -> Observe ping requests and replies within WireShark 
</p>
<br />

<p>
<img width="1440" alt="STEP 3 nonstop ping from windows to ubun" src="https://github.com/user-attachments/assets/5c838741-c40d-4410-8f4f-7171df1ca292"/>
<img width="1440" alt="step 3 network settings click linux vm" src="https://github.com/user-attachments/assets/118a15cc-f961-4471-be95-40670b9bfebc"/>
<img width="1440" alt="click linux-vm-nsg" src="https://github.com/user-attachments/assets/4411254a-0548-4d86-b0e5-4ca8be67c1e2"/>
<img width="1440" alt="disabled incoming icmp traffic STEP 3 13 A" src="https://github.com/user-attachments/assets/40971a30-6c7f-46d9-8bbd-72ca5b87521c"/>
<img width="1439" alt="SHOWS THAT THE 290 IS defaulted for icmp traffic rules set" src="https://github.com/user-attachments/assets/5bcc1376-477c-4cd4-b398-d52b66c715c5"/>
<img width="1440" alt="request timed out bc icmp has been denied" src="https://github.com/user-attachments/assets/6a10e58e-a9eb-4167-9069-be642e9ebb3c"/>
</p>
<p>
Configuring a Firewall -> Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM(ping 10.2.0.5 -t) -> enter -> Next, Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic -> Go back to Azure portal -> click on the Linux VM -> Networking -> Network settings -> Network Security Group -> click linux-vm-nsg -> settings -> Inbound Security rules to create a rule for traffic coming inbound to the VM ->  Add -> Make sure the(Destination port ranges) is an asterik * simply means any port -> protocol "ICMPv4" -> Action -> Deny -> priority(290) which will put that rule above the default rule -> Add(Can see now inbound security rules set) -> Back to PSh you will see in the terminal that "Request Timed out" means that the rule has been applied 
</p>
<br />

<p>
<img width="1440" alt="ssh traffic only STEP 3 16" src="https://github.com/user-attachments/assets/bd54c5f6-5d18-435e-81a6-0765dbaf64bc"/>
<img width="1431" alt="using login name with private ip address STEP 3" src="https://github.com/user-attachments/assets/a33400e6-5978-4e6d-86ba-57d94da8d2e9"/>
<img width="1440" alt="last step for observing ssh  STEP 3" src="https://github.com/user-attachments/assets/6bde4dc4-f677-4c3c-9cbf-6e602fe2a197"/>
<img width="1440" alt="typed in cmds in wireshark STEP 3" src="https://github.com/user-attachments/assets/5e951e19-52c7-49c0-8567-a0a3bb6fe439"/>
</p>
<p>
Now to Observe SSH traffic -> Login to Microsoft Azure, Turn on the Vms you'll need to use, Login to the specified VM using the given Public Ip address if VM has been deleted -> Back in Wireshark, start a packet capture up -> Filter for SSH traffic only -> From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address) -> observe its SSH traffic -> Then I typed commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark 
</p>
<br />

<p>
<img width="1440" alt="dhcp step 1" src="https://github.com/user-attachments/assets/5d6475ca-6539-45d1-b28d-b62290d1cacb"/>
<img width="1439" alt="ipconfig:renew  typed dhcp" src="https://github.com/user-attachments/assets/5e470573-d1a1-4932-a654-fde0bbfc7889"/>
</p>
<p>
Now to Observe DHCP traffic -> Back in Wireshark, filter for DHCP traffic only -> From my Windows 10 VM, I attempted to issue my VM a new IP address from the command line, by typing in command line "ipconfig /new" -> Observe the DHCP traffic appearing in WireShark
</p>
<br />

<p>
<img width="1440" alt="dns lookup" src="https://github.com/user-attachments/assets/48846433-e11e-43fc-96b9-d3aa9212d51f"/>
</p>
<p>
Now to Observe DNS traffic -> Back in Wireshark, filter for DNS traffic only -> From my Windows 10 VM within a command line, I used nslookup to see what google.com and disney.com’s IP addresses were -> Observe the DNS traffic being shown in WireShark
</p>  
<br />




