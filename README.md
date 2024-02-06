<h2>Description</h2>
TryHackMe PS Eclipse room. 
Use Splunk to investigate the ransomware activity.
<br />


<h2>Room walk-through:</h2>

<p align="center">
  Scenario: <br/>
  As a Security Operations Center (SOC) Analyst employed at Random SSB Merge Security Services, a provider of security solutions, I am tasked with investigating events reported by customers. Recently, a client reached out via email requesting an analysis of the events that transpired on Keegan's machine on Monday, May 16, 2022.

According to the client's report, Keegan's machine appears to be functioning normally, but some files possess unfamiliar file extensions. While the machine remains operational, there are indications of aberrant file activity, suggesting partial compromise. It's noteworthy that the machine has not been completely locked down, a common occurrence in ransomware attacks where access to the entire file system is restricted.

The presence of unusual file extensions raises concerns of a potential ransomware attempt targeting Keegan's device. This situation warrants thorough investigation to ascertain the extent of the compromise and to implement appropriate remedial measures to safeguard Keegan's data and system integrity.<br/>
<p align="center">
<img src="https://i.imgur.com/Ac8w0m5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <p align="center">
Before proceeding with the analysis of events in Splunk, it's prudent to acknowledge the possibility of ransomware infiltration on Keegan's machine. It is plausible that ransomware has accessed the machine, either through a direct download or as a result of a compromise. In scenarios like this, it's common for the machine to be compromised first, followed by the download of the ransomware payload to encrypt files.

To gain clarity on the sequence of events, we will leverage Splunk for analysis. By selecting the "all time" timeframe and opting to retrieve all available data, even in the absence of a specific index, we can comprehensively examine the data ingested into Splunk. This approach will enable us to pinpoint any anomalous activities and identify potential indicators of compromise.

  <br />
  <br />
  <p align="center">
<img src="https://i.imgur.com/3ZQnfhj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Let's examine the Intrusion Prevention System (IPS) logs. In the destination IPs, we observe two IP addresses that are generating the most events. Notably, these IPs are communicating over ports 443 (HTTPS). This provides us with valuable insights into the activity under investigation, which primarily revolves around network connections on the machine.

Focusing specifically on network connections, we observe a significant number of events occurring over the HTTPS (HTTP Secure) protocol. However, it's important to note that there are also events related to port 443 , which we will not delve into at this moment.<br/>
<br/>
<br/>
<p align="center">
<p align="center">
<br />
<br />
The analysis reveals that the source machine we are investigating has been actively engaging in both downloading and retrieving content from the internet. This bidirectional communication indicates a significant level of activity involving external resources.

Upon clicking on port 443, we observe numerous destination IP addresses, representing the various endpoints the machine was communicating with. Among these, one particular IP address stands out, possibly indicating the presence of an attacker or a Command and Control (C2) server. However, it's essential to emphasize that we cannot definitively confirm this at this stage; we are merely examining the available data and discussing potential scenarios.

Notably, despite the numerous destination IP addresses, the source IP remains consistent, reflecting the machine's IP address initiating these connections. This consistency suggests a pattern of behavior emanating from the source machine, further warranting detailed scrutiny and investigation. <br/>
<p align="center">
<img src="https://i.imgur.com/U9pzolZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/U9pzolZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
