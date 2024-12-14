<p align="center">

</p>

![image](https://github.com/user-attachments/assets/2124d289-817e-4382-bc52-c3c0edff8d58)


<h1>Basics of networking tutorial</h1>
This tutorial outlines the use of Wireshark to observe various network protocols and activities.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- PowerShell

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Linux Ubuntu

<h2>High-Level Deployment and Configuration Steps</h2>

- Download/Install Wireshark
- Disable ICMP traffic in the Firewall
- Capture and observe traffic through Wireshark
  
  

<h2> Introduction</h2>

<p>This tutorial provides a step-by-step guide to installing and using Wireshark to capture and analyze traffic for five key networking protocols: ICMP, SSH, DHCP, DNS, and RDP.  By the end of this guide, you will understand how to identify and interpret packet data for these protocols, equipping you with the foundational skills to analyze network performance, detect issues, and enhance security. </p>

<h2>Download and Install Wireshark</h2>
<p>
Wireshark is a powerful network protocol analyzer that allows IT professionals to monitor and troubleshoot network activity in real-time. To begin, we will use Remote Desktop to connect to your Windows 10 Virtual Machine. Once connected, the first step is to install Wireshark, a powerful tool for network traffic analysis. After installation, open Wireshark and initiate a packet capture session by selecting your active network interface.
</p>

 ![lab2_dl_wireshark](https://github.com/user-attachments/assets/5135657e-334c-45ff-a3ce-a0dcacf9fc15)


<p>

</p>
<br />

![lab2_dl_wireshark2](https://github.com/user-attachments/assets/43942620-90e6-4348-a0a0-ec208f3fc8cb)

<br/>

<p>
Below is the view you will see in Wireshark once you choose to start capturing packets.
</p>
<br/>

![wireshark_running](https://github.com/user-attachments/assets/9d36dffd-d1e7-4b08-8a51-a5ceb276f14b)

<br/>
<p>
Next, you'll want to filter for ICMP traffic within Wireshark. This will allow you to focus on specific network communications, such as ping requests and replies. Now, to test ICMP communication, retrieve the private IP address of your Ubuntu VM (referred to as linux-vm) and attempt to ping it from the Windows 10 VM. As you issue the ping command, return to Wireshark and observe the captured traffic to see the ICMP requests and responses. For an additional test, open the command line or PowerShell on your Windows 10 VM and try to ping a public website (e.g., www.google.com). Once again, monitor the ICMP traffic in Wireshark. You should see the outgoing ping requests to the public IP address of the website and the corresponding replies. This exercise demonstrates how ICMP traffic is used for network diagnostics and how Wireshark can capture and analyze these packets in real time.
</p>
<br />



![first_ping_vm2](https://github.com/user-attachments/assets/a924a8a1-d1e7-4d96-bb20-29bfbfb94072)
<br/>

<p> For our next activity, we'll initiate a perpetual ping from your Windows 10 Virtual Machine (VM) to your Ubuntu VM. This can be done by opening the Command Line or PowerShell on the Windows 10 VM and running the command:</p>

<p> ping [Ubuntu VM IP] -t</p>

<p> The -t flag ensures that the ping command continues indefinitely until manually stopped. While the ping is running, open Wireshark on your Windows 10 VM to capture and observe the ICMP traffic in real-time.</p>

<br/>

<p>Now, to test how network security rules affect communication, navigate to the Network Security Group (NSG) associated with your Ubuntu VM. Locate the rule for inbound ICMP traffic and disable it. This action effectively blocks ping requests from reaching the Ubuntu VM. </p>
<br/>





![DenyICMP_VM2](https://github.com/user-attachments/assets/4ba59405-4e28-4371-96f9-b0489767e906)

<br/>

<p> Return to the Windows 10 VM and observe what happens in both the command line and Wireshark. In the command line, you will see that the ping requests are no longer receiving responses—indicating that the Ubuntu VM is no longer reachable via ICMP. In Wireshark, you'll notice the outgoing ping requests but no corresponding replies.</p>
<br/>

![VM1_afterICMP_denied](https://github.com/user-attachments/assets/601b7d96-2dbc-4507-aa6d-d9fb164ec66f)

<br/>

<p> Next, return to the Network Security Group settings and re-enable ICMP traffic. Once this change is applied, go back to your Windows 10 VM and observe the ICMP traffic in both Wireshark and the command line. The ping activity should resume successfully, with replies now being received from the Ubuntu VM.

Finally, stop the ping activity by pressing Ctrl + C in the command line. This exercise demonstrates how Network Security Groups control inbound traffic and how changes in security settings impact network diagnostics, allowing you to monitor the effects using tools like Wireshark.</p>

<br/>


![VM2_allowICMP](https://github.com/user-attachments/assets/6d1837c3-49b4-4d6c-93a1-60545ca8c51c)

<br/>

![VM1_afterICMP_allowed](https://github.com/user-attachments/assets/c55d4068-3794-4703-8803-ea194d4dec2f)

<br/>

<h3> Monitoring SSH Traffic Using Wireshark</h3>
<p> First, log back into your Windows 10 Virtual Machine (VM) using Remote Desktop. Once inside, open Wireshark and start a new packet capture on the active network interface. To narrow down the captured traffic, apply a filter for SSH by typing ssh into the Wireshark filter bar. This ensures you'll only see traffic related to Secure Shell (SSH) connections.</p>

<br/>



![15-16_WSpakcetforSSH](https://github.com/user-attachments/assets/76d558db-d754-413b-986f-99f5e91e466e)

<br/>

<p> With Wireshark capturing SSH traffic, open PowerShell on your Windows 10 VM. Here, you'll initiate an SSH connection to your Ubuntu Virtual Machine (VM) using its private IP address. Use the command:

ssh labuser@[Ubuntu VM private IP]

When prompted, enter the password for the labuser account on the Ubuntu VM. Once authenticated, you'll gain access to the Linux shell. Try typing a few Linux commands within this SSH session, such as checking the directory with ls or viewing system information with uname -a. As you interact with the SSH session, return to Wireshark to observe the stream of encrypted SSH traffic between your Windows and Ubuntu VMs.
</p>

<br/>

![18a_SSHintoLinuxVM](https://github.com/user-attachments/assets/f5dd38e0-7af4-421f-adf0-562c6dd7af13)

<br/>

<p> You’ll notice that, unlike protocols like HTTP or ICMP, the SSH packets captured by Wireshark appear encrypted, ensuring that any sensitive information (like passwords) is not visible in plain text. This demonstrates the security SSH provides when communicating over a network.</p>

<br/>

![18b_commandsVIAssh](https://github.com/user-attachments/assets/dec682de-90c3-4102-aa52-8c077b12c88c)

<br/>

<p> When you're finished, exit the SSH session by typing:
exit

and pressing Enter. This will terminate the connection, and you'll see the corresponding closure of the SSH session in Wireshark. This exercise highlights how SSH secures data transmission and how Wireshark can be used to monitor encrypted network traffic for troubleshooting and analysis.
</p>

<br/>

<h3> Analyzing DHCP Traffic Using Wireshark</h3>
<br/>
<p> To explore how the Dynamic Host Configuration Protocol (DHCP) works, we'll use Wireshark to capture DHCP traffic while renewing the IP address of your Windows 10 Virtual Machine (VM). First, ensure you are logged into your Windows 10 VM, then open Wireshark and start a new packet capture session. This time, apply a filter for DHCP traffic by typing dhcp into the filter bar. This will help you focus solely on the DHCP communication between your VM and the network's DHCP server.</p>
<br/>
<p> Now, to request a new IP address for your Windows 10 VM, open PowerShell with administrator privileges. In PowerShell, run the following command:

ipconfig /renew

This command instructs your Windows 10 VM to reach out to the DHCP server and request a new IP lease. In response, the DHCP server will assign an IP address, along with other network configuration details, to your VM.
</p>

<br/>

![19-20_DHCPtraffic](https://github.com/user-attachments/assets/d85d880f-4f50-4424-b812-ab8c22eedf91)

<br/>

<h3>Analyzing DNS Traffic with Wireshark </h3>
<p>In this tutorial, you'll explore how the Domain Name System (DNS) works by capturing DNS traffic using Wireshark while resolving domain names. Start by logging into your Windows 10 Virtual Machine (VM) and opening Wireshark. Begin a new packet capture session, then set a filter for DNS traffic by entering dns in the filter bar. This will allow you to focus solely on DNS requests and responses during the exercise.

Now, to test DNS resolution, open the Command Line or PowerShell on your Windows 10 VM. You'll use the nslookup tool, which queries DNS servers to find the IP addresses associated with domain names. Enter the following commands:

nslookup google.com
nslookup disney.com

</p>
<br/>

![21-22DNStraffic](https://github.com/user-attachments/assets/394e5438-45e7-4f66-9f44-99a0c9558130)

<br/>

<p> As you run each command, nslookup sends a query to the DNS server configured on your network to resolve the domain name into an IP address. Switch back to Wireshark and observe the captured DNS traffic. You should see:

  1.DNS Query: The Windows 10 VM sends a query packet to the DNS server asking for the IP address associated with the domain (e.g., google.com or disney.com).

  2.DNS Response: The DNS server responds with one or more IP addresses associated with the requested domain name.

These DNS packets provide a clear example of how domain names are resolved into IP addresses, allowing browsers and other applications to connect to websites. In Wireshark, you'll notice that DNS traffic typically uses UDP on port 53 for queries and responses, although occasionally it may use TCP for larger responses or if the connection is more persistent.

This exercise highlights how DNS facilitates web browsing and network communications, and demonstrates how Wireshark can be used to observe and troubleshoot DNS-related network issues.

</p>
<br/>

<h3> Monitoring RDP Traffic with Wireshark</h3>

<p>To understand how Remote Desktop Protocol (RDP) traffic functions, you'll use Wireshark to observe network communication while you're remotely connected to another computer. Start by logging into your Windows 10 Virtual Machine (VM) where you've established a Remote Desktop session. In Wireshark, begin a new packet capture session and apply a filter specifically for RDP traffic by entering:

tcp.port == 3389

 </p>

 <br/>
 <p> This filter will focus on capturing only the traffic related to your active RDP session, which uses TCP port 3389.

As soon as you apply the filter, you will notice an immediate and continuous stream of traffic in Wireshark, even if you aren't actively moving the mouse or typing. This non-stop data flow might seem surprising at first, but it reveals a key characteristic of how the RDP protocol works. Unlike protocols that only transmit data when specific actions are taken (like HTTP or DNS), RDP is designed to constantly stream data between the client (your Windows 10 VM) and the server (the remote computer).</p>

<br/> 

![23-24TCPtraffic](https://github.com/user-attachments/assets/bffb0890-1387-473c-8a40-7b7fb6096f37)

<br/>
<p>
The reason for this continuous transmission is that RDP provides a live feed of the remote desktop environment, similar to a video stream. It sends updates for everything displayed on the remote screen, even if nothing appears to change. This ensures that your Remote Desktop session remains fully synchronized in real-time, allowing you to see any updates on the remote screen without delay.

In Wireshark, you'll observe that the captured RDP traffic shows packets being sent back and forth continuously, indicating the constant exchange of data required to keep the remote session active and responsive. This exercise highlights how protocols like RDP differ from others by maintaining a persistent connection to support real-time interaction.
</p>
<br /># Networking-Basics
Tutorial of basic networking protocols
