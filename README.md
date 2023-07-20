<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Microsoft Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create our Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic

<h2>Actions and Observations</h2>

Step 1: Create 2 virtual machines in Azure, one using Windows 10 as an image and the other using Ubuntu Server (Linux) as its image
<p>
<img Screenshot 2023-07-20 at 9 33 32 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/480ce8d3-8f56-44c3-8323-f9d716403a97">
</p>
<img Screenshot 2023-07-20 at 9 34 38 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/51833a82-eae6-48b6-ae25-661e26b0df61">
<p>
<img Screenshot 2023-07-20 at 9 26 31 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/85822e29-eb84-464d-80df-31621e45c374">
</p>
<img Screenshot 2023-07-20 at 9 26 42 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/4d604795-7ad4-4d87-9ac3-fa50f6e86f31">
<p>
As you can see, one VM is running on Windows and the other is running on Linux
</p>
<img Screenshot 2023-07-20 at 9 46 21 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/af1b1977-341e-4eb2-b599-38f13ca834df">
<p>
<br />

Step 2: Open VM-1 via Microsoft Remote Desktop application using its public IP address - Paste this <a href="https://2.na.dl.wireshark.org/win64/Wireshark-win64-4.0.7.exe">Wireshark </a> link into the web browser to download the program
<p>
<img Screenshot 2023-07-20 at 10 04 28 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/020285f4-727e-4af9-b9cd-30b57432e43e">
</p>
Agree to all of the standard installation steps and Install
<p>
<img Screenshot 2023-07-20 at 10 13 21 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/634e7b78-d1a9-46be-8883-8d4648916dd6">
</p>
Once installed, search Wireshark in the search bar and open the application
<p>
<img Screenshot 2023-07-20 at 10 16 46 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/fbd1d474-185f-4f3d-9f9b-cede991bae96">
</p>
In Wireshark, select Ethernet then click the blue fin in the top left to start capturing packets and show us the live traffic that is happening on our virtual machine
<p>
<img Screenshot 2023-07-20 at 10 19 18 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/7d9d8e50-d196-4f40-8906-8e3fdbb7e1d8">
</p>
<img Screenshot 2023-07-20 at 10 20 36 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/0cab1b4b-963a-4678-aad1-1df96b8dfe45">
<p>
<br />

Step 3: In the search bar search "icmp" to filter for only icmp and notice that there isn't any traffic being captured
<p>
<img Screenshot 2023-07-20 at 10 24 40 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/ea1deb6e-10a3-4e2c-9bf1-4759bba155b2">
</p>
Navigate back to Azure and copy VM-2's private IP address which will be used to ping over in VM-1
<p>
<img Screenshot 2023-07-20 at 10 29 18 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/86a1493f-0ea9-4bc6-aa21-f362b4b2029b">
</p>
With the private IP address of VM-2, head back to VM-1 - Open PowerShell - Type "ping xx.x.x.x" - Notice the icmp packets that are captured on Wireshark as well as the communication between the two IP addresses
<p>
<img Screenshot 2023-07-20 at 10 33 48 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/4aaf4bb5-151f-4d21-9f20-1bea0badf124">
</p>
The same applies if we type in "ping www.google.com -4" into PowerShell. Our IP address (10.0.0.5) sends a request to www.google.com and it recieves a response from one of Google's IP addresses, indicating that there is a connection between the two
<p>
<img Screenshot 2023-07-20 at 10 46 07 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/b1cf6e73-55c4-420d-afa8-5a23080bd52f">
</p>
<br />

Step 4: Initiate a perpetual (nonstop) ping from VM-1 to VM-2
<p>
First clear the data that is in Wireshark by clicking the green fin the top left corner and "Continue without Saving"
</p>
<img Screenshot 2023-07-20 at 10 51 59 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/bef32b76-f5b4-46eb-9de0-c035d32d404e">
<p>
In PowerShell type the command "ping xx.x.x.x -t" using VM-2's private IP address and you'll notice the perpetual ping being initiated
</p>
<img Screenshot 2023-07-20 at 10 57 20 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/275c69a2-b4c5-4b51-a023-986fdbb3f3af">
<p>
<br />

Step 5: Configure VM-2's firewall to not accept icmp traffic then reallow icmp traffic
<p>
Navigate to Azure and open up VM-2's Network Security Group 
</p>
<img Screenshot 2023-07-20 at 11 15 38 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/efea4baa-b2db-411a-97ee-7bb3b8bdf507">
<p>
Select Inbound Security rules - Add - Under Protocol select ICMP - Under action select Deny
</p>
<img Screenshot 2023-07-20 at 11 19 29 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/b084cb24-5159-46d2-8c5f-16569a1d2051">
<p>
Switch back over to VM-1 and on PowerShell you will notice the Request Timed Out and you'll notice that there is only requests from VM-1 to VM-2 with no replies coming from VM-2 since its firewall is now blocking any icmp connections
</p>
<img Screenshot 2023-07-20 at 11 27 37 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/172ff11a-0c1d-44c6-8ac9-afcb46b636c6">
<p>
Now that we have an idea of how the firewall works, we can reallow icmp traffic to VM-2 by allowing the icmp in the network security group rule that we created to block icmp in Azure
</p>
<img Screenshot 2023-07-20 at 11 32 50 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/40f3e0a8-8652-4be5-9287-d631491e2abd">
<p>
Back over in VM-1 we can now see that we can receive replies from VM-2 on both PowerShell and Wireshark once again
</p>
<img Screenshot 2023-07-20 at 11 35 34 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/01f48bbd-403d-432e-8eac-3f238693b19e">
<p>
<br />

Step 6: Observe SSH traffic
<p>
On VM-1 close and reopen PowerShell to clear its contents and clear the contents in Wireshark as well - Start a new search for SSH or tcp.port == 22 (interchangeable) in Wireshark
</p>
<img Screenshot 2023-07-20 at 11 43 43 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/744d3837-735f-4c30-88b3-d7c4b20c61fb">
<p>
In PowerShell type in the command "ssh Labuser@10.0.0.6" (Labuser is the username we created when registering VM-1 and VM-2 | 10.0.0.6 is VM-2's private IP address) - Type Yes when prompted - Type in the password created for VM-2 (It will not show while typing) - You are now connected to VM-2's command line while in VM-1
</p>
<img Screenshot 2023-07-20 at 11 52 20 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/591e7594-a1d7-4262-a78c-6df55027e434">
<p>
To exit VM-2 in PowerShell type "exit" and you'll notice that you are back as the Labuser for VM-1
</p>
<img Screenshot 2023-07-20 at 11 59 01 AM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/8dfb7d05-20a5-4c7c-a6e9-73d867b56246">
<p>
<br />

Step 7: Observe DHCP traffic
<p>
Clear the contents in PowerShell and Wireshark - Start a new search for DHCP in Wireshark
</p>
In PowerShell type the command "ipconfig /renew" and you will see DHCP traffic being populated in Wireshark and PowerShell that displays the IP address reissued to us
<p>
<img Screenshot 2023-07-20 at 12 09 08 PM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/01e93a80-3676-4168-a1a7-a744b258dbbb">
</p>
<br />

Step 8: Observe DNS traffic
<p>
Clear the contents in PowerShell and Wireshark - Start a new search for DNS or udp.port == 53 (interchangable) in Wireshark
</p>
<img Screenshot 2023-07-20 at 12 13 21 PM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/dc4c6948-fb67-48a7-944e-59f05b37be39">
<p>
In PowerShell type the command "nslookup www.google.com" which will show us traffic in PowerShell and Wireshark of VM-1 asking the DNS server what Google's IP address is 
</p>
<img Screenshot 2023-07-20 at 12 19 55 PM" src="https://github.com/areyes302/Azure-Network-Protocols/assets/139584521/4fdc7e5d-755c-4577-a291-071035b0c75a">
<p>
<br />

CONGRATULATIONS 🎉, you have successfully observed various network traffic happening between different virtual networks!





























