# SIEM-Elastic: Alert Triage and Log Analysis &nbsp;&nbsp; <img width="80" height="80" alt="elastic logo" src="https://github.com/user-attachments/assets/3daa5922-d257-4a0e-b376-0e9dfb06b8eb" />

Analyze logs in Elastic to investigate alerts and identify potential threats.

<details>
  <summary><b>Scenario</b></summary>
  
This project simulates a real-life incident response scenario where I acted as an on-call SOC analyst for a Managed Service Provider (MSP). Using Elastic (Kibana), I performed high-pressure alert triage and investigated multiple security triggers originating from a client’s Windows and IIS servers.

I began by reviewing the SOC Dashboard (Fig. 1), which displayed several high-severity alerts, including **Web Requests Indicating File Upload** and **New User Account Created**. The presence of **"Unusual Command-Line Behavior"** indicated a high probability of a successful system compromise.

<img width="1119" height="273" alt="image" src="https://github.com/user-attachments/assets/d3811a74-e83f-460e-8caf-523cbf04defc" />

*Figure 1: SOC Dashboard*

I filtered the web server logs to correlate the alerts with incoming network traffic. By drilling down into the client.ip field, I successfully identified the Source IP (Attacker) as **203.0.113.55**.

<img width="1456" height="464" alt="image" src="https://github.com/user-attachments/assets/2dff31e2-2426-4529-af77-f2e71893e7d8" />

*Figure 2: Source IP (Attacker)*

</details>

<details>
  <summary><b>Investigating Web Attacks</b></summary>
  
The alert below shows the IP address 203.0.113.55 made **POST** requests to the page **proxyLogon.ecp**. 

<img width="634" height="262" alt="image" src="https://github.com/user-attachments/assets/92c0ddd9-6094-4ef0-8cf3-671b302f5f6d" />

*Figure 3: SOC Alert: Web Requests Indicating File Upload*

The first goal is to visualize all web requests from the IP address. Figure 4 (below) shows that the attacker utilized the user-agent **python-requests/2.25.1** to make the POST requests, which are related to the **ProxyLogon** vulnerability.

<img width="1454" height="457" alt="image" src="https://github.com/user-attachments/assets/b6383d97-09a5-4641-b797-c09f92170f01" />

*Figure 4: user-agent*

The following alert (Fig. 5) comes in 7 minutes after the first one and appears suspicious because the **cmd=** parameter is a hallmark of web shell activity. This alert will be investigated to identify whether or not it is a True Positive.

<img width="626" height="253" alt="image" src="https://github.com/user-attachments/assets/d86d8ea0-6ba5-46e3-a7a0-a33fbbed9f66" />

*Figure 5: SOC Alert: GET Requests to ASPX File with Query Parameters*

Figure 6 (below) shows some of the commands that were run using the web shell in the url.path, these results indicate a **breach** and should be escalated!

<img width="1452" height="629" alt="image" src="https://github.com/user-attachments/assets/d4bb3283-17e3-41d5-8f6c-3674b92042b3" />

*Figure 6: SOC Alert: url.path showing commands*

</details>

<details>
  <summary><b>Uncovering Account Activity</b></summary>

Suspicious network activity has been identified and now it is time to pivot to host-base evidence to identify if the attacker has progressed. Figure 7 shows that the **Administrator** account has accessed the server outside of regular business hours, thus this should be investigated.
  
  <img width="624" height="265" alt="image" src="https://github.com/user-attachments/assets/f476750e-18e6-4d67-b8ef-bd894979ad79" />
  
*Figure 7: SOC Alert: Administrator Access Outside of Business Hours*

I focused the search based on the time of the event, Windows Security Even ID **4624** (Logon), client's hostname and the Administrator user.

<img width="1456" height="348" alt="image" src="https://github.com/user-attachments/assets/78c5e409-97c5-47bd-8ba2-114d4bafc3be" />

*Figure 8: winlog.logon.type showing attacker accessed the system remotely*

Figure 9 (below) further shows the Administrator logon that triggered a normal Windows session initialization process chain, which can be verified in the dashboard. The two queries confirmed that the logon took place, but the current evidence isn’t sufficient to determine whether the activity is malicious.

<img width="1454" height="586" alt="image" src="https://github.com/user-attachments/assets/2a9ad003-0673-4138-b1e9-f51b9bb74b10" />

*Figure 9: Administrator's process.command_line*

The next critical alert in Fig. 10 (below) shows that the Administrator created a new user account, which is suspicious and warrants further investigation.

<img width="617" height="244" alt="image" src="https://github.com/user-attachments/assets/00e9de89-9f30-42d3-978b-4d31152b6a1d" />

*Figure 10: SOC Alert: New User Account Created*

Figure 11 (beloow) shows the activity of the newly created user account. Figure 12, after adjusting queries, showed the username of the account to be **svc_backup**.

<img width="1450" height="562" alt="image" src="https://github.com/user-attachments/assets/6817adc1-7a32-47e8-89a6-eb0737181517" />

*Figure 11: New User **(Account Management)** activity*

<img width="1455" height="364" alt="image" src="https://github.com/user-attachments/assets/ee376cca-7ea9-4012-af0f-d6f53b31cae0" />

*Figure 12: Name of the new user account*

</details>

<details>
  <summary><b>Exposing Command Execution</b></summary>

  This next crticial alert (Fig. 13) flags suspicious command-line usage from the same Administrator account. 

  <img width="628" height="256" alt="image" src="https://github.com/user-attachments/assets/17d17151-bcb7-45fe-934f-d14e38b43ae0" />

  *Figure 13: SOC Alert: Unusual Command-Line Behavior: Privilege Changes*

The first steps I did was to investigate what child process was launched by **cmd.exe** and to verify who launched it and why. Figure 14 (below) shows that the new account used the **net.exe** process to add the new account to three groups. 

I confirmed that the Administrator used CMD to create a new user and modify security groups. However, I still need to determine whether the activity stopped there or continued using the newly created account. To do this, I’ll continue analyzing Sysmon logs and also review PowerShell logs for further evidence.

  <img width="1454" height="463" alt="image" src="https://github.com/user-attachments/assets/cec9f98b-7a2b-4cc1-b99e-30bedac87a18" />

  *Figure 14: New account added to three groups*




</details>
