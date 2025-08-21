#  BazTech Inc. SOC Simulation Report

**Project Title:** A SOC in a Segmented Network  
**Analyst:** Justice Akason  
**Date:** August 2025  
**Environment:** Segmented SOC Lab (Wazuh, Kali Linux, Ubuntu, Windows 10)

---

##  Executive Summary

This report outlines a simulated SSH brute-force attack conducted within BazTech Inc.’s segmented SOC lab. The objective was to assess detection capabilities, incident response workflows, and compliance readiness using the Wazuh SIEM platform.

The exercise validates:
- Operational use of threat intelligence
- Real-time log correlation across hosts
- Incident response under realistic conditions
- Clear documentation for technical and non-technical stakeholders

---

##  Lab Architecture & Segmentation

The lab was built to replicate enterprise network segmentation and simulate adversarial behavior across security zones.

| Component         | Role          | IP Address        | Key Functionality                             |
|-------------------|---------------|-------------------|-----------------------------------------------|
| Kali Linux        | Attacker      | 192.168.***.***   | Hydra brute-force tool for SSH                |
| Ubuntu Server     | DMZ Target    | 192.168.***.***   | SSH service with logging enabled              |
| Windows 10 Agent  | Internal Host | 192.168.***.***   | Event log generation, lateral movement test   |
| Wazuh Manager     | SIEM          | 192.168.***.***   | Centralized log aggregation and alerting      |

<img width="759" height="449" alt="image" src="https://github.com/user-attachments/assets/17dace5c-558f-47cc-8436-5befc9447bcc" />



###  Segmentation Validation

Traceroute and firewall rules confirmed isolation between attacker, DMZ, and internal zones. Only authorized connections were permitted across interfaces.

---

##  Attack Simulation Overview

- **Attack Type:** SSH Brute-force  
- **Tool Used:** Hydra  
- **Target:** Ubuntu Server (192.168.***.***)  
- **Command Executed:**
```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.***.***
```

<img width="652" height="515" alt="Screenshot 2025-08-19 212746" src="https://github.com/user-attachments/assets/6c1ecc1f-5b9c-49a4-b133-4e2ce18ec71d" />


---

##  Observations & Alerts

- **Log Volume:** `/var/log/auth.log` recorded 22,853 failed login attempts.
- **Wazuh Alert:** Rule ID `5715` triggered on brute-force detection.
- **Classification:** MITRE ATT&CK mapping to **T1110 - Brute Force**

### Wazuh Alert Summary:
| Severity | Count |
|----------|-------|
| Medium   | 113   |
| Low      | 186   |
| Critical | 0     |

<img width="1910" height="422" alt="Screenshot 2025-08-19 213258" src="https://github.com/user-attachments/assets/9432ec2b-76dd-4ad4-b4a8-dfde93851e46" />


###  Security Configuration Assessment (SCA)

- 47 failed hardening checks detected
- Weak SSH configuration
- Missing auditing policies

---

##  MITRE ATT&CK Mapping

| Technique ID | Name                        | Phase               |
|--------------|-----------------------------|---------------------|
| T1110        | Brute Force                 | Credential Access   |
| T1003        | Credential Dumping          | Credential Access   |
| T1082        | System Information Discovery| Discovery           |
| T1105        | Remote File Copy            | Command & Control   |
| T1011        | Data Exfiltration           | Exfiltration        |

---

##  Log Analysis & Forensic Evidence

- **Sample Log Entry:**
```log
Aug 19 22:15:01 ubuntu sshd[1234]: Failed password for root from 192.168.***.*** port 54*** ssh2
```

<img width="1011" height="297" alt="Screenshot 2025-08-19 234636" src="https://github.com/user-attachments/assets/1676e28a-10f4-4008-9159-f31d832710fb" />


- **Live Authentication Events:**

<img width="1000" height="754" alt="Screenshot 2025-08-19 215010" src="https://github.com/user-attachments/assets/2795d452-a184-41fc-b2a2-2eaf3a5be201" />


- **Wazuh Alert JSON:**
```json
{
  "rule": {
    "id": "5715",
    "level": 10,
    "description": "Possible SSH brute-force attack"
  },
  "srcip": "192.168.***.***",
  "location": "/var/log/auth.log"
}
```

![Wazuh JSON](https://github.com/user-attachments/assets/6425ebd0-e01c-45b2-a1ff-00930bfdbaef)

---

##  Visual Evidence

Annotated screenshots of the Wazuh dashboard, alert breakdown, and SCA results were compiled as part of the stakeholder report.

![Stakeholder View](https://github.com/user-attachments/assets/3dc0f4be-b06b-40f6-90f0-5a8263466d19)

---

##  Mitigation & Hardening Actions

| Action                 | Description                                          |
|------------------------|------------------------------------------------------|
| IP Blocking            | `iptables -A INPUT -s 192.168.***.*** -j DROP`       |
| SSH Hardening          | Disabled password auth; enforced key-based login     |
| Network Access Control | Restricted SSH to trusted IP ranges                  |
| Wazuh Rule Tuning      | Elevated brute-force alerts; enabled active response |
| SCA Remediation        | Applied CIS benchmarks; reduced failed checks        |

---

##  Documentation & Deliverables

Deliverables included:

- Annotated diagrams of network and attack flow
- Markdown checklist of vulnerabilities and remediations
- Executive summary for non-technical stakeholders
- MITRE mapping table for audit and compliance
- Log evidence and alert breakdown for forensic validation

---

##  Lessons Learned

- **Detection Depth:** Wazuh was effective but required tuning for actionable clarity.
- **Network Segmentation:** Firewalls prevented lateral movement — segmentation success.
- **Stakeholder Communication:** Visual and organized reporting enhanced impact.
- **Analyst Growth:** Hands-on experience sharpened threat emulation and documentation skills.

---

##  Conclusion

This simulation showcases the full lifecycle of SOC operations — from segmentation and adversary emulation to detection, response, and reporting. I demonstrated strong technical execution, clear communication, and alignment with frameworks like ISO 27001 and GDPR.
