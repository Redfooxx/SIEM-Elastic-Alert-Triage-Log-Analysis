# SIEM-Elastic: Alert Triage and Log Analysis
Analyze logs in Elastic to investigate alerts and identify potential threats.

<details>
  <summary><b>Scenario</b></summary>
  
This project simulates a real-life incident response scenario where I acted as an on-call SOC analyst for a Managed Service Provider (MSP). Using Elastic (Kibana), I performed high-pressure alert triage and investigated multiple security triggers originating from a client’s Windows and IIS servers.

I began by reviewing the SOC Dashboard (Fig. 1), which displayed several high-severity alerts, including Web Requests Indicating File Upload and New User Account Created. The presence of **"Unusual Command-Line Behavior"** indicated a high probability of a successful system compromise.

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

The first goal is to visualize all web requests from the IP address. Figure 2 shows that the attacker utlized the user-agent **python-requests/2.25.1** to make the POST requests, which are related to the **ProxyLogon** vulnerability.

<img width="1454" height="457" alt="image" src="https://github.com/user-attachments/assets/b6383d97-09a5-4641-b797-c09f92170f01" />

*Figure 4: user-agent*

The following alert (Fig 5) comes in 7 minutese after the first one and appears suspicious because the **cmd=** parameter is a hallmakr of web shell activity. This alert will be investigated to identify whether or not it is a True Positive.

<img width="626" height="253" alt="image" src="https://github.com/user-attachments/assets/d86d8ea0-6ba5-46e3-a7a0-a33fbbed9f66" />

*Figure 5: SOC Alert: GET Requests to ASPX File with Query Parameters*

Fig.6 shows some of the commands that were run using the web shell in the url.path.

<img width="1452" height="629" alt="image" src="https://github.com/user-attachments/assets/d4bb3283-17e3-41d5-8f6c-3674b92042b3" />

*Figure 6: SOC Alert: url.path showing commands*

</details>

<details>
  <summary><b>Uncovering Account Activity</b></summary>
  
![image](https://github.com/user-attachments/assets/6575376c-19e0-4c1d-82cb-dafb87c154c2)

</details>

<details>
  <summary><b>Exposing Command Execution</b></summary>
  
![image](https://github.com/user-attachments/assets/6575376c-19e0-4c1d-82cb-dafb87c154c2)

</details>
