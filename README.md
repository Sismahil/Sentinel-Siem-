# Sentinel-Siem-
![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/fd5fd083-97d4-4b7a-927d-daec24664981)

<h1>Introduction</h1>
This lab shows how i set up Azure Sentinel (SIEM) and connected it to a live virtual machine acting as a honey pot. I will observe live attacks (RDP Brute Force) from all around the world. I also used a custom Powershell script to look up the attackers Geolocation information and plot it on the Azure Sentinel Map.  <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Microsoft Sentinel
- PowerShell
- RDC

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Setups</h2>

<p>
  
![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/e167bbbd-2eb2-481b-a06a-43fed7d5edec)

</p>
<p>
Step 1: I Created a VM named Honeypot by adding an adminstrator account usernames and password. I also edited the NIC network secourity group from basic to advanced while making the firewall open publicly which will allow all traffic from the internet into our VM only for inspection only for the sake of this tutorial. The reason for this is to make the VM discoverable or susceptible to attacks.
</p>
<br />


<p>
  
![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/2dd61b31-23c2-4ebe-a114-ef75a4d5ac32)

</p>
<p>
Step 2: I created a custom log anylysis workspace to ingest logs from the VM and determine the geolocation on the map.
</p>
<p>
Step 3: I went to security center to enable the ability to gathered logs from VM into the log analysis workspace
</p>
Step 4: went to work analysis workspace and connected it to VM.
</p>
<p>
Step 5: I setup Sentinel (SIEM) by creating a new one, added log analysis workspace (LawhoneyPot)
</p>
<p>
Step 6: I connceted the VM by copying the Public IP Adress of my VM and pasted into RDC(Remote Desktop Connection)
</p>
  Note: I intentionally put the wrong credentials when logging into my RDC so that i could obserse it in the log
<p>
Step 7: I open event viewer to inspect the failed credentials attempt.
<p>
  
  ![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/58c00893-162c-4e88-a183-e92f60cf12d8)

</p>
<p>
Step 8: I turned off the the windows defender firewall in my VM so that it would be susceptible to attacks quickly from people all over the world. Again, i did this only for the sake of this Tutorial.
</p>
Step 9: I copied a powershell script https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1 and pasted it into Window PowerShell ISE on my VM. while inputing my API key i got from https://ipgeolocation.io/ after signing up.
</p>
<p>
  
![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/102a1dfa-aa8f-48d1-8ec0-e3193bfc935a)
</p>
<p>
Step 10: created a custom log inside our Log Analysis Worskspace that will allow me to bring the logs with geolocation into log analysis workspace. To do this, I copied the logs from the powerrshell in our VM and pasted it and saved it on notepad in order to be able to browse for the file in our azure. Then select "windows" on collection paths, and put the diectory of the failed logs in the path.
</p>
<p>

![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/4f648355-6f2d-4bcd-87ac-4d7827bb03fa)
</p>
<p>
Step 11: went to log and run a query of the created custom logs. This might take a while for log analysis workspace and VM to sync in for the custom logs. 
</p>
 <p> 
   
![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/452ace08-39ad-44eb-92ef-5884ca2b1638)
</p>
<p>
Step 12: Extracted it and customize into longitide, latitude,country in order to be able plot it the map. After that, I went to Microsoft Sentinel (SIEM) on Azure to inspects incidence on my SIEM. went to workbook and add query. However, I made sure the powershell script is running updates on bad actors trying to exploit the VM because it is susceptible to attacks. Then gave it some time to gather more brute force attempts
</p>
<p>

  ![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/bd755155-a5e1-46da-b4c3-7e06c2850667)
  ![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/4945f42f-d8bd-439c-abff-402d0b47b5f7)

</p>
<h1>Observation</h1>
<p>

![image](https://github.com/Sismahil/Sentinel-Siem-/assets/121772702/ac91977c-2fdf-454e-9c61-17bce9604ec3)
  
</p>
NOTE: After leaving the Powershell running for like 12hrs, I noticed bunch of attacks all over the world with totall of 7000+ attacks. Majority from philpine and Russia aggregating up to 4500 attacks. The Takeaway is as soon as anything is vulneranble on the internet, people will always try to exploit it for their benefit regardless of big or small your organiation is. Another takeway would be to consider using a strong password or multi-authentication approach to avoid brute force attack on gaining access to the host. Never use the password of admistrator because it is likely to be guessed right away by bad actors.

