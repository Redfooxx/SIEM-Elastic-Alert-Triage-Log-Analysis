# SIEM-Elastic: Alert Triage and Log Analysis
Analyze logs in Elastic to investigate alerts and identify potential threats.

<details>
  <summary><b>Scenario</b></summary>
This project simulates a real-life incident response scenario where I acted as an on-call SOC analyst for a Managed Service Provider (MSP). Using Elastic (Kibana), I performed high-pressure alert triage and investigated multiple security triggers originating from a client’s Windows and IIS servers.

<img width="1119" height="273" alt="image" src="https://github.com/user-attachments/assets/d3811a74-e83f-460e-8caf-523cbf04defc" />

*Figure 1: SOC Dashboard*

I filtered the web server logs to correlate the alerts with incoming network traffic. By drilling down into the client.ip field, I successfully identified the Source IP (Attacker) as 203.0.113.55.
<img width="975" height="277" alt="image" src="https://github.com/user-attachments/assets/8c6d5905-7bd0-4d3e-b79b-5da187f36b83" />
*Figure 2: Source IP (Attacker)*
</details>

<details>
  <summary><b>Investigating Web Attacks</b></summary>
  
<img width="634" height="262" alt="image" src="https://github.com/user-attachments/assets/92c0ddd9-6094-4ef0-8cf3-671b302f5f6d" />


</details>

<details>
  <summary><b>Uncovering Account Activity</b></summary>
  
![image](https://github.com/user-attachments/assets/6575376c-19e0-4c1d-82cb-dafb87c154c2)

</details>

<details>
  <summary><b>Exposing Command Execution</b></summary>
  
![image](https://github.com/user-attachments/assets/6575376c-19e0-4c1d-82cb-dafb87c154c2)

</details>
