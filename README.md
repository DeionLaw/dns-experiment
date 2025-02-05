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

<h2>Pre-Reqs</h2>
- 2 machines on a local network, I will be using info and machines from (https://github.com/DeionLaw/configure-ad)

<h2>Actions and Observations</h2>

![Screenshot 2025-02-05 123716](https://github.com/user-attachments/assets/47aedc39-1c97-4839-92c8-afc23ea5c2b3)

<p>
  First, I am going to log into both of the machines as the Jane Admin account we created. She has the admin permissions that are needed in this environment. 
</p>

<p>
  INSIDE CLIENT MACHINE: I am going to open Windows PowerShell, inside I am going to type 'ping ThePentagon', this is a random name I chose, you can ping your own chosen name if following along. You will see that the ping request has failed. This shows us that we do not have an IP Address for 'ThePentagon' in our local DNS cache, local host file, or our DNS Server (remember we set this to be our domain controller machine). Those three places are checked in that order, for speed, all of which have no idea what the IP Address of 'ThePentagon' is.
</p>

---

![Screenshot 2025-02-05 123912](https://github.com/user-attachments/assets/c6896215-742d-428c-bd2f-9df848d2aa2e)
![Screenshot 2025-02-05 124438](https://github.com/user-attachments/assets/cb47f187-31ae-4f61-8cae-999152d54bad)


<p>
  To view the current DNS cache you can type 'ipconfig /displaydns'. You can scroll through the conversions that your machine has saved locally. You can see that 'ThePentagon' is no where to be found.
</p>

<p>
  To view the local host file, you can find this file at This PC > Windows (C:) > Windows > System32 > drivers > etc > hosts. If you open this file in notepad (open notepad as administrator and then find the file to edit it) you should see something similiar to the second screenshot.
</p>

---
![Screenshot 2025-02-05 130805](https://github.com/user-attachments/assets/4f6f9dfc-8b86-4437-9ceb-97008ccf2136)
![Screenshot 2025-02-05 131546](https://github.com/user-attachments/assets/fcffc2ad-238d-4a12-a888-4bfc9f9cfe0d)



<p>
  If we were to add the loopback address (127.0.0.1) and a name for the IP Address, 'WhiteHouse' is what I will name it.
</p>

<p>
  You can see now in the second screenshot that pinging 'WhiteHouse' now results with some results. This shows us that our machine now knows what 'WhiteHouse' is.
</p>

---


