# BazTech Inc. SOC Simulation Report
## Project Title: A SOC in a Segmented Network
## Analyst: Justice Akason
## Date: August 2025
## Environment: Segmented SOC Lab ( Wazuh, Kali, Ubuntu, Windows 10)

## Executive Summary
This report details a simulated SSH brute-force attack within BazTech Inc.’s virtual SOC environment. The exercise validates detection capabilities, incident response workflows, and compliance alignment using Wazuh. It demonstrates my ability to operationalize threat intelligence, correlate logs across platforms, and produce stakeholder-ready documentation under deadline pressure.

## Lab Architecture & Segmentation
The SOC lab was designed to mirror enterprise segmentation and simulate adversary behaviour across zones:
Component	Role	IP Address	Key Functionality
Kali Linux	Attacker	192.168.15.5	Hydra brute-force tool targeting SSH
Ubuntu Server	DMZ Target	192.168.15.7	SSH service with logging enabled
Windows 10 Agent	Internal Host	192.168.15.9	Event log generation, lateral movement test
Wazuh Manager	SIEM	192.168.15.6	Centralized log analysis and alerting

<img width="759" height="449" alt="image" src="https://github.com/user-attachments/assets/4bb1f26f-30db-4c3d-b625-8ce84ee235c9" />

 
## Segmentation Validation:
Firewall rules and traceroute tests confirmed isolation between attacker, DMZ, and internal zones. Only authorized traffic was permitted across interfaces.

## Attack Simulation Details
Attack Type: SSH Brute-force
Tool Used: Hydra
Target: Ubuntu Server (192.168.15.7)

<img width="901" height="493" alt="image" src="https://github.com/user-attachments/assets/a494c19b-3ba0-4eb0-a460-e5ecdc321852" />
 

## Command Executed:
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.15.7 

## Observed Behaviour:
•	/var/log/auth.log recorded 22,853 failed login attempts.
•	Wazuh Agent parsed logs and triggered Rule ID 5715.
•	Alerts were classified as Brute-force attempt with MITRE mapping to T1110.

## Wazuh Detection & Alerting
Dashboard Highlights:
•	Total Alerts: 299
o	Medium: 113
o	Low: 186
o	Critical: 0

<img width="901" height="493" alt="image" src="https://github.com/user-attachments/assets/67bebdc5-209f-4e78-9892-ec89de842f1a" />
 
•	Security Configuration Assessment (SCA):
•	47 failed hardening checks on Ubuntu
•	Weak SSH configuration, missing audit policies

## MITRE ATT&CK Mapping:
Technique ID	Name	Phase
T1110	Brute Force	Credential Access
T1003	Credential Dumping	Credential Access
T1082	System Information Discovery	Discovery
T1105	Remote File Copy	Command & Control
T1011	Data Exfiltration	Exfiltration

## Log Analysis & Evidence
Sample Log Entry from /var/log/auth.log:
Aug 19 22:15:01 ubuntu sshd[1234]: Failed password for root from 192.168.15.5 port 54321 ssh2 

<img width="940" height="251" alt="image" src="https://github.com/user-attachments/assets/50017dc5-7315-41b9-abed-ed9775194809" />
 

## Real-Time Authentication Events: Ubuntu Terminal Output

<img width="868" height="544" alt="image" src="https://github.com/user-attachments/assets/ea4438f9-901d-4524-b2fe-2125ec025511" />
 

## Wazuh Alert JSON:
{ "rule": { "id": "5715", "level": 10, "description": "Possible SSH brute-force attack" }, "srcip": "192.168.15.5", "location": "/var/log/auth.log" } 

<img width="875" height="536" alt="image" src="https://github.com/user-attachments/assets/6425ebd0-e01c-45b2-a1ff-00930bfdbaef" />
 

## Visual Evidence:
Annotated screenshots of Wazuh dashboard, alert breakdown, and SCA results were captured and included in stakeholder documentation.

<img width="836" height="510" alt="image" src="https://github.com/user-attachments/assets/3dc0f4be-b06b-40f6-90f0-5a8263466d19" />
 


## Mitigation & Hardening Actions
Action Taken	Description
IP Blocking	  iptables -A INPUT -s 192.168.15.5 -j DROP
SSH Hardening	  Disabled password auth; enforced key-based login
Network Access Control	  Restricted SSH to trusted IP ranges via Firewall
Wazuh Rule Tuning	  Elevated brute-force alerts; enabled active response
SCA Remediation	  Applied CIS benchmarks; reduced failed checks

## Documentation & Stakeholder Deliverables
I produced the following artifacts:
•	Annotated diagrams of attack flow and segmentation
•	Markdown checklist of vulnerabilities and remediation steps
•	Executive summary tailored for non-technical stakeholders
•	MITRE mapping table for compliance and audit teams
•	Log excerpts and alert evidence for forensic validation

## Lessons Learned & Analyst Reflection
•	Detection Depth: Wazuh effectively correlated logs across zones, but tuning was required to reduce noise and elevate critical alerts.
•	Segmentation Success: firewall rules prevented lateral movement, validating network isolation.
•	Documentation Impact: Clear, visual reporting accelerated stakeholder understanding and decision-making.
•	Analyst Growth: This simulation sharpened my skills in adversary emulation, SIEM tuning, and stakeholder communication under pressure.

## Conclusion
This capstone simulation demonstrates my ability to design, execute, and document a full-cycle SOC workflow—from attack emulation to detection, mitigation, and reporting. The deliverables reflect operational clarity, technical rigor, and strategic alignment with compliance frameworks like ISO 27001 and GDPR.

