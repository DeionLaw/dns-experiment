<p align="center">
![dns-logo](https://github.com/user-attachments/assets/8fbdb96e-a6bd-4349-9e68-3f37c74c594a)
</p>

<h1>Inspecting and Experimenting with DNS</h1>
In this tutorial, we experiment and explore DNS.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- PowerShell              

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server

<h2>Pre-Reqs</h2>
- 2 machines on a local network, I will be using info and VMs from (https://github.com/DeionLaw/configure-ad)

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

![Screenshot 2025-02-05 133334](https://github.com/user-attachments/assets/e57c695b-4039-47c6-94e2-6b7d9d63690a)


<p>
  Now that we know we can add information to our DNS logs manually, we can check to see first off that 'nslookup ThePentagon' reports back that it can not be found.
</p>

---

![Screenshot 2025-02-05 134114](https://github.com/user-attachments/assets/28733e7e-f8b9-45ff-a6ac-9862474a874a)
![Screenshot 2025-02-05 134240](https://github.com/user-attachments/assets/2f69b33d-128e-4f5a-8eb2-04d1f6d3f32d)


<p>
  Now, INSIDE THE DOMAIN CONTROLLER: We can open the 'DNS' app, which will open 'DNS Manager'. Inside, we will navigate the dropdowns to find DC-1 (or your device name) > Forward Lookup Zones > mydomain.com and in there we will right click and select 'New Host (A or AAAA)...). In the new pop up we will set the name to 'ThePentagon' and set the IP address to the domain controllers private IP address (10.0.0.4 for me, you can find this by typing ipconfig into powershell.)
</p>

<p>
  In the second screenshot, you can see 'ThePentagon' was added to this list inside the DNS server (we set the DNS server to our domain controller)
</p>

---

![Screenshot 2025-02-05 134452](https://github.com/user-attachments/assets/0cf4a8db-3fe5-43c9-96c3-129288b5d863)

<p>
Now, INSIDE THE CLIENT MACHINE: We can see that pinging 'ThePentagon' now pings to a device under our control, congrats on the power you have achieved.
</p>

---
![Screenshot 2025-02-05 134800](https://github.com/user-attachments/assets/e3675fa2-94b3-4795-90a2-df0d4a1a5686)


<p>
  Now, BACK INSIDE THE DOMAIN CONTROLLER: We will change the A record in the DNS Manager of 'ThePentagon' to ping to 8.8.8.8 (The primary DNS Server for Google).
</p>

---
![Screenshot 2025-02-05 134914](https://github.com/user-attachments/assets/02576c8e-3f67-4ab1-969a-a26bf4d55d5a)


<p>
  Finally, BACK IN THE CLIENT MACHINE: We are going to ping 'ThePentagon' in PowerShell. You will notice that pinging 'ThePentagon' after changing the A record on the DNS server still ends up pinging the private IP address (no, this is not the same screenshot). Remember that the computer checks first the local DNS cache, THEN the host file, THEN finally it'll default out to checking with the DNS server.
</p>

---

![Screenshot 2025-02-05 135450](https://github.com/user-attachments/assets/4fbe7daf-05d6-4ea5-ad19-bc85c8b3a838)


<p>
  So, still on the client machine, we are going to type 'ipconfig /flushdns' into powershell. This will clear the DNS cache that is on our machine. Now, we type 'ipconfig /displaydns' and you can see that everything has been cleared except the 'WhiteHouse' that I manually added to the host file.
</p>

---

![Screenshot 2025-02-05 135658](https://github.com/user-attachments/assets/5b7db920-6f64-401d-8176-1c41d399009e)

<p>
  Now, you can see that 'ThePentagon' now pings to the '8.8.8.8' address because it has nothing in the cache or the host file for 'ThePentagon' and it defaulted to the DNS server, where we set 'ThePentagon' to the IP address of 8.8.8.8.
</p>


