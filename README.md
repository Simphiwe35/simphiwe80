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
<br />
<br />
The analysis reveals that the source machine we are investigating has been actively engaging in both downloading and retrieving content from the internet. This bidirectional communication indicates a significant level of activity involving external resources.

Upon clicking on port 443, we observe numerous destination IP addresses, representing the various endpoints the machine was communicating with. Among these, one particular IP address stands out, possibly indicating the presence of an attacker or a Command and Control (C2) server. However, it's essential to emphasize that we cannot definitively confirm this at this stage; we are merely examining the available data and discussing potential scenarios.

Notably, despite the numerous destination IP addresses, the source IP remains consistent, reflecting the machine's IP address initiating these connections. This consistency suggests a pattern of behavior emanating from the source machine, further warranting detailed scrutiny and investigation. <br/>
<p align="center">
<img src="https://i.imgur.com/U9pzolZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  It's understandable to find the question regarding the downloaded binary suspicious. While it might seem evident that the binary in question is the one downloaded from the identified IP address, making assumptions without concrete evidence is not advisable. Conclusions must be drawn based on tangible evidence rather than conjecture.

Therefore, it's crucial to backtrack and carefully examine the available data to ascertain the origin and nature of the downloaded binary. This process involves thorough analysis of network traffic logs, including source and destination IP addresses, timestamps, and any associated metadata. Additionally, correlating this information with other relevant indicators of compromise (IOCs) can provide valuable insights into the incident.

By adhering to a rigorous investigative approach and relying on verifiable evidence, we can accurately determine the name and origin of the downloaded binary, thereby facilitating effective incident response and mitigation efforts.<br />
<p align="center">
<img src="https://i.imgur.com/RGCNdQG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
  <br />
  You're absolutely correct. The question specifies that the binary was downloaded, indicating that it could have been retrieved through either port 443 or port 80. Given the disparity in event counts between the two ports, with port 443 exhibiting significantly higher activity (296 events), it's prudent to focus our investigation on this port initially.

Upon examining the data associated with port 443, we may indeed encounter a binary with a peculiar name. However, it's essential to exercise caution and refrain from jumping to conclusions prematurely. Instead, we must diligently search for additional clues and corroborating evidence to confirm whether this binary is indeed the one we are seeking.

By scrutinizing the surrounding context, such as timestamps, source and destination IP addresses, and any associated metadata, we can construct a comprehensive picture of the event. This meticulous approach ensures that our conclusions are grounded in solid evidence, thereby enhancing the accuracy and effectiveness of our investigative efforts.<br />
<p align="center">
<img src="https://i.imgur.com/mhjG1U8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  
Analyzing the data associated with the TCP protocol, we observe that the communication is not occurring over HTTPS but rather over plain TCP. Further examination reveals three distinct paths involved: one under "temp," another associated with Windows Defender, and a third linked to OneDrive.

Upon closer inspection, we identify three executables related to Port 443. However, our investigation shouldn't conclude here; we must also explore the activities associated with Port 80.

Switching our focus to Port 80, we observe a different set of executables, with PowerShell being the sole executable identified. While this provides us with partial insight, it's crucial to delve deeper by examining the specifics of PowerShell's involvement in this context. Let's proceed by clicking on PowerShell to gather more information.<br />

<p align="center">
<img src="https://i.imgur.com/qqgu3JG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
  <br />

Based on the information gathered, it appears that PowerShell was utilized to establish a connection over Port 80 to a destination server. However, the specific command executed remains unknown at this point, leaving a gap in our understanding of the incident.

To further investigate, let's return to the data related to PowerShell, where we have identified 281 events involving this executable. By examining the interesting fields associated with these events, we aim to uncover additional details that could shed light on the command used.

Highlighting the "command line" field among the 34 available fields, as well as the "description" field, may provide valuable insights into the nature of the PowerShell activity. Additionally, exploring the "parent command line" field could offer context regarding the origin or trigger of the PowerShell execution.

By scrutinizing the command lines associated with PowerShell, we may uncover suspicious or anomalous activities that warrant further investigation. Let's proceed with this approach to unravel the reasons behind the utilization of PowerShell over Port 80 and ascertain any potential threats or compromises.<br />
<p align="center">
<img src="https://i.imgur.com/kNo1qoL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  
Upon examining the PowerShell activity further, we have discovered a Base64-encoded string embedded within a 64-bit string. To decode this string and unveil its contents, we will utilize CyberChef.

After pasting the Base64 string into CyberChef and selecting UTF as the decoding format, the decoded text reveals the partial command used.
This command indicates an attempt to bypass real-time monitoring by setting the execution policy, followed by the execution of a script retrieved from the specified URL. The presence of such a command confirms our suspicions regarding the suspicious nature of the file and its potential threat to the system's integrity.<br />

<p align="center">
<img src="https://i.imgur.com/dVbRgvc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/TD1Jtc7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To answer the question about the executable name used to download the suspicious binary, we need to identify the full path of the PowerShell executable. While we know that PowerShell was used to execute the command to download the binary, we can find the full path of PowerShell by examining the "image" field.

By selecting the "image" field, we can retrieve the full path of the PowerShell executable. This path will provide us with the information needed to answer the question accurately.

Let's proceed by accessing the "image" field to obtain the full path of the PowerShell executable and use that information to answer the question about the executable name.<br />
<p align="center">
<img src="https://i.imgur.com/bMzKj97.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  
To answer the questions:

What command was executed to configure the suspicious binary to run with elevated privileges?
The command executed to configure the suspicious binary to run with elevated privileges was.

What permissions will the suspicious binary run as?
The suspicious binary will run with elevated privileges, as configured by the command mentioned above.

What was the user?
The user associated with the execution of the command is "NT Authority\System."

These answers can be derived from the Base64-encoded string extracted from the PowerShell activity, as it contains the command executed and the user context in which it was executed.<br />

<p align="center">
<img src="https://i.imgur.com/3sBYjxv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />
<img src="https://i.imgur.com/cMm8g8V.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <br />
<br />
question
suspicious binary connected to a remote server what address did it connect to
add HTTP to your answer so that's
suspicious binary guys it connected to a
distillation server let's find out
  
Upon examining the "query name" field, we observe five events associated with DNS queries. Let's delve into one of these events to gain further insight.

The "query name" field reveals domain names or servers that the system attempted to resolve through DNS queries. In this context, it appears that these queries are related to communication with a potential Command and Control (C2) server or the domain from which the suspicious binary was retrieved.

One such domain identified in the query is "ngrock.io," which appears to be the main domain involved. Additionally, there is another extension or subdomain associated with this domain, indicating a connection destination for the executable after its download.

Based on this analysis, it's apparent that these DNS queries are significant in understanding the network activity surrounding the suspicious binary and its communication with external entities.<br />
<p align="center">
<img src="https://i.imgur.com/kMccyGF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />
<img src="https://i.imgur.com/ZGYB08M.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />

Based on the information provided, it appears that a partial script was downloaded to the same location as the suspicious binary. This partial script may have played a role in facilitating the encryption of files on the system, although it may not be the ransomware itself.

To search for the partial script, we can simplify the process by filtering events for files with the ".ps1" extension, as partial scripts typically have this extension in PowerShell environments. By doing so, we can identify all instances of partial scripts in the events data.<br />
<p align="center">
<img src="https://i.imgur.com/SzyH5iL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />
<img src="https://i.imgur.com/816hNy2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
question.<br />
judging by the other names of the other Scripts the malicious script was flagged as
 malicious what do you think was the actual name of script?<br />
  
Based on the information provided, it seems that the malicious script, although flagged as malicious, may have been given a different name by the attacker to conceal its true nature. To determine the actual name of the script, we can retrieve the MD5 hash associated with the script and use it to query VirusTotal, a service that provides information on file hashes and their reputations.

By submitting the MD5 hash to VirusTotal, we can obtain details about the script, including its actual name. Upon reviewing the details, it appears that the real name of the script is "Black Sun," as suggested by the hash. However, it's possible that the attacker used a different name for the script to avoid detection or mislead analysts.

This underscores the importance of conducting thorough investigations and utilizing multiple sources of information, such as file hashes and online threat intelligence platforms, to accurately identify and assess potential threats.<br />
<p align="center">
<img src="https://i.imgur.com/YDE1w6k.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />
<img src="https://i.imgur.com/AzIhGIX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

To uncover the full extent of the situation, it appears that the malicious binary was utilized to retrieve the partial script responsible for encrypting the files on the system. Now, the focus shifts to understanding the attacker's motives.

One common indicator of ransomware activity is the presence of ransom notes, typically stored as text files. To determine if a ransom note exists, we can search for files with the ".txt" extension, as these files often contain ransom demands or instructions from attackers.

Upon conducting the search for text files, we discover three events. These events likely correspond to the presence of ransom notes on the system. One of the notes is located in the "Downloads" directory.

This discovery suggests that the attacker may indeed be pursuing a ransom-based attack, emphasizing the need for swift action to mitigate the impact and prevent further harm to the system and its data.<br />

<p align="center">
<img src="https://i.imgur.com/OBJmHg1.png"80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://i.imgur.com/W8HbB5x.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />  
To address the last question regarding the location of the image file that was saved to replace the user's desktop wallpaper, we can search for image files on the system. In this case, we'll start by searching for files with the ".jpg" extension, as this is a common format for image files. We may also search for other common image formats such as PNG or BMP, depending on the attacker's preferences.

Upon searching for JPG files, we discover two events indicating the presence of image files. These events provide the directory paths where the images are stored. By examining these paths, we can determine the full path of the image file that was used to replace the desktop wallpaper.

This completes the challenge, as we have successfully identified the paths of both the ransom notes and the image file used to replace the desktop wallpaper, providing valuable insights into the attacker's actions and the compromise of the system.<br /> 
<p align="center">
<img src="https://i.imgur.com/0DE0gG1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
<br />
<img src="https://i.imgur.com/wAgCpQq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>


