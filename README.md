<h1>Azure-Security-Operations-Threat-Detection</h1>

<h2>Description</h2>
We set up an essential home SOC in Azure from scratch in this project. Using a free Azure subscription, we walk through creating a virtual machine (VM), opening it to the internet as a honeypot, and forwarding logs to a central repository. We then integrate Microsoft Sentinel to analyze real-world attack data.
<br />


<h2>Languages and Utilities Used</h2>

- <b>KQL</b> 
- <b>Microsoft Azure</b>
- <b>Microsoft Sentinal</b>
- <b>Log Analytics</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Walk-through:</h2>

<h4> Step 1: Setup Azure Subscription </h4>

- Create Free Azure Subscription: https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account.
  <h5> *** Note: If Azure doesn’t let you create a free account, you can use a paid subscription but be mindful of shutting down/deleting your resources when you are done. </h5>
- After your Subscription is created, you can log in at: https://portal.azure.com

<p align="left">
Sign up: </br>
<img src="https://i.imgur.com/OkQElHX.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
</p>

<h4> Step 2: Create the honeypot (Azure Virtual Machine) </h4>

- Go to: https://portal.azure.com
- Search for resource group
<p align="left">
Create Resource Group: </br>
<img src="https://i.imgur.com/XhVuG75.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Choose Subscription --> Resource Name --> Region:  <br/>
<img src="https://i.imgur.com/78goWOt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
</p>

- Search for Virtual Network 

<p align="left">
Virtual Network: </br>
<img src="https://i.imgur.com/6jq9D3A.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Create Virtual Network:  <br/>
<img src="https://i.imgur.com/7ogy6Yj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Choose subscription --> resource group (same resource group)--> assign a name --> choose the same region --> review and create --> create:  <br/>
<img src="https://i.imgur.com/9CxVRpl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

- Search for virtual machines

<p align="left">
Search Virtual Machine: </br>
<img src="https://i.imgur.com/4DBKmZp.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Create Azure Virtual Machine:  <br/>
<img src="https://i.imgur.com/oH71ad0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Choose subscription --> Resource Group (must be same resource group) --> Assign a machine name --> Choose Region (ideally near your onw location)  :  <br/>
<img src="https://i.imgur.com/25Ih1cu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Choose Window 10 pro for Image --> Choose size (be mindful to shutdown Virtual Machine after lab) :  <br/>
<img src="https://i.imgur.com/hSLAyWm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create username --> Create password: (Remember username and password)  <br/>
<img src="https://i.imgur.com/z9HPwJg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm Agreement --> Next: Disks <br/>
<img src="https://i.imgur.com/mKsVyD2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Choose OS Disk size --> Choose OS Disk type --> Next: Networking <br/>
<img src="https://i.imgur.com/ODqdFi0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Choose Virtual Network (one we created earlier) --> Leave subnet as default <br/>
<img src="https://i.imgur.com/joPUG0e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click on Delete public IP and NIC when VM is deleted --> Next: Management <br/>
<img src="https://i.imgur.com/wAFEDyf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Skip to monitoring --> Disable Diagnostic --> Review & Create --> Create <br/>
<img src="https://i.imgur.com/w4bMifF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<img src="https://i.imgur.com/0xZCYOs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

- Disable the Network Security Group for your virtual machine and create a rule that allows all traffic inbound
<p align="left">
Go to Resource Group and Click on Network security Group: </br>
<img src="https://i.imgur.com/85dpZvH.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Delete 300 inbound security rule to allow the attack <br/>
<img src="https://i.imgur.com/BOJzr9w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Go to Setting --> Inbound security rule --> Create <br/>
<img src="https://i.imgur.com/Y9qe9hE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Change source (any) --> Source Port Ranges (*) --> Destination (Any) --> Service (Custom) --> Destination port ranges (*) --> Protocol (Any) --> Action (Allow) <br/>
<img src="https://i.imgur.com/3TIw4FK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Change Priority (100) --> Assign name of rule --> Add <br/>
<img src="https://i.imgur.com/KwkT9h9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

- Log into your virtual machine and turn off the Windows firewall (start -> wf.msc -> properties -> all off)
<p align="left">
Go to Virtual Machine --> your virtual machine --> copy Public IP Address: </br>
<img src="https://i.imgur.com/2RclG47.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Go to Search bar (in your actual PC) --> Type "Remote Desktop Connection" --> paste Public IP Address --> Connect : </br>
<img src="https://i.imgur.com/jkR2Q5l.png" height="80%" width="80%" alt="Azure security Operation"/>
<img src="https://i.imgur.com/1iIfL3e.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Enter Username and Password: </br>
<img src="https://i.imgur.com/RLLIgw4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/NNKRltV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Go to Search bar (in your Virtual PC) --> Type "wf.msc" --> Click on "Window Defender firewall Properties" --> Turn firwall off on Domain, Private and Public profile  : </br>
<img src="https://i.imgur.com/jkR2Q5l.png" height="80%" width="80%" alt="Azure security Operation"/>
<img src="https://i.imgur.com/EUAIGkJ.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Go to Seach bar (in your actual PC) --> Type "Command Prompt" --> Type " Ping your virtual machine public ip address (ping 40.82.205.87)" </br>
<img src="https://i.imgur.com/5fVbAqF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/4i8Jdx1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

<h4> Step 3: Logging into the VM and inspecting logs </h4>
- Log off your virtual machine

- Try to fail log in as "employee" 3 - 4 times (with another username for Event Viewer)

- Login to your virtual machine

- Open Up Event Viewer and inspect the security logs.

- Fail login should be visible in the security logs ( EventID 4625 ). We can find source network address (where attacker is from)

<img src="https://i.imgur.com/zXv5jeZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/qDL8I6K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/K0N5YM7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/LhBLJgW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

- Next, we are going to create a central log repository called a LAW

<h4> Step 4: Log Forwarding and KQL </h4>

- Create Log Analytics Workspace
- Create a Sentinel Instance and connect it to Log Analytics
- Configure the “Windows Security Events via AMA” connector
- Create the DCR within sentinel, watch for extension creation

<p align="left">
Search for Log Analytics Workspaces </br>
<img src="https://i.imgur.com/HN0ur6U.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Click on Create <br/>
<img src="https://i.imgur.com/yRpUAek.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Choose subscription --> Assign Resource group --> Assign Name --> Choose region --> Create + Review --> Create: <br/>
<img src="https://i.imgur.com/PaC8BBC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/gVepJq4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Once Log Analytics is created, search for "Sentinel" --> Create <br/>
<img src="https://i.imgur.com/HsZWBCp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Zra33NK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select Log Analystic --> Add --> content hub --> select " Window Security Events" --> Manage <br/>
<img src="https://i.imgur.com/zNlLM6f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/vlFF1RN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/rMA1RkS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select " Window Security Event via AMA" --> Open Connector page --> Create Data Connection Rule: <br/>
<img src="https://i.imgur.com/oKf96HG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/A7Nt2Zr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Assign Rule name --> Choose Subscription --> Assign Reosurce Group --> Choose Virtual Machine --> Next: <br/>
<img src="https://i.imgur.com/sDcbvdg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/zby9Z1f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
In collecting, Choose "All Security Event" --> Review + Create --> Create: <br/>
<img src="https://i.imgur.com/hybtEZa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/zby9Z1f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
In Virtual Machine, Sentinel should be in "Extension + Application" --> Create: <br/>
<img src="https://i.imgur.com/naJEye5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>


- Query for logs within the LAW
- Now we can query the Log Analytics workspace and the SIEM to sentinel directly, which we will do soon.
       <h5> Note: Querying logs in here is a very important skill that you MUST have if you want to work in security operations. Depending on where you work, you need to know SQL, KQL, or SPL, but these are all the same thing. If you know one, you can easily learn the others </h5>
- Observe some of your VM logs:

  SecurityEvent
  
  | where EventID == 4625

<h4> Step 5: Log Enrichment and Finding Location Data </h4>

- Observe the SecurityEvent logs in the Log Analytics Workspace; there is no location data, only IP address, which we can use to derive the location data.
- We will import a spreadsheet (as a “Sentinel Watchlist”) that contains geographic information for each block of IP addresses.

[geoip-summarized](https://drive.google.com/file/d/1akZFLmTWRxHECPQaYIHnIfTwPZTctRnz/view?usp=sharing)

- Within Sentinel, create the watchlist:

Name/Alias: geoip

Source type: Local File

Number of lines before row: 0

Search Key: network

- Allow the watchlist to fully import, there should be a total of roughly 55,000 rows.
- In real life, this location data would come from a live source or it would be updated automatically on the back end by your service provider.
- Observe the logs now have geographic information, so you can see where the attacks are coming from


<h4> Step 6: Creating Attack Map </h4>

- Within Sentine, create a new Workbook.
- Delete the prepopulated elements and add a “Query” element.
- Go to the advanced editor tab, and paste the JSON
- [Workbook (Attack map)](https://drive.google.com/file/d/1FLUIkzdbk4ypYg-OEza9e4ZJ9w7kYnu7/view?usp=sharing))
- Observe the query
- Observe the map settings
- Observe the map
- Finish!


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
