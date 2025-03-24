<h1>Azure-Security-Operations-Threat-Detection</h1>

 ### [YouTube Walkthrough (In prog)](https://youtube.com)

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
Choose Subscription, name, and Region:  <br/>
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
Choose resource group, give a name, choose same region and review and create:  <br/>
<img src="https://i.imgur.com/9CxVRpl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

- Search for virtual machines

<p align="left">
Search Virtual Machine: </br>
<img src="https://i.imgur.com/4DBKmZp.png" height="80%" width="80%" alt="Azure security Operation"/>
<br />
<br />
Create virtual then Azure Virtual Machine:  <br/>
<img src="https://i.imgur.com/oH71ad0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

- Create a new Windows 10 virtual machine (choose an appropriate size). You notice the monthly cost of leaving the VM on 24/7.
  <h5> Be mindful of shutting this off when you are done </h5>
  <h5> Remember the Username and Password </h5>
- Go to the Network Security Group for your virtual machine and create a rule that allows all traffic inbound
- Log into your virtual machine and turn off the Windows firewall (start -> wf.msc -> properties -> all off)

<h4> Step 3: Logging into the VM and inspecting logs </h4>

- Try to fail log in as "employee" 3 - 4 times (with another username for Event Viewer)
- Login to your virtual machine
- Open Up Event Viewer and inspect the security logs.
- Fail login should be visible in the security logs ( EventID 4625 ).
- Next, we are going to create a central log repository called a LAW
  
<h4> Step 4: Log Forwarding and KQL </h4>

- Create Log Analytics Workspace
- Create a Sentinel Instance and connect it to Log Analytics
- Configure the “Windows Security Events via AMA” connector
- Create the DCR within sentinel, watch for extension creation
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
