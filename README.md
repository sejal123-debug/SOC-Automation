# SOC Automation Lab Environment Setup

A complete on-premises SOC (Security Operations Center) lab built for DFIR (Digital Forensics & Incident Response), threat detection, and log analysis. This environment includes Active Directory, Windows 10, and Kali Linux machines, with centralized log collection and simulated attacks for detection engineering and investigation.

## Table of Contents

- [ Project Objective](#-project-objective)
- [ Lab Components](#-lab-components)
- [ System Requirements](#-system-requirements)
- [ Network Diagram](#-network-diagram)
- [ IP Address Scheme](#-ip-address-scheme)
- [ Tools & Technologies](#-tools--technologies)
- [ Setup Instructions](#-setup-instructions)
- [ Attack Simulations](#-attack-simulations)
- [ Log Analysis in Splunk](#-log-analysis-in-splunk)
- [ Resources](#-resources)
- [ Future Improvements](#-future-improvements)

##  Project Objective

To build a self-contained lab for SOC automation and incident response training. The goal is to:
- Understand and visualize log flows.
- Simulate real-world attacks.
- Analyze security events.
- Automate detection and response playbooks.

## Lab Components

| Component         | Purpose                        |
|------------------|---------------------------------|
| Active Directory | Centralized authentication      |
| Splunk Server     | Log collection & analysis       |
| Windows 10 Target | Victim for attack simulations   |
| Kali Linux        | Attacker machine                |
| Sysmon + Splunk UF | Event logging on endpoints     |
| Atomic Red Team  | Attack simulation framework      |


##  System Requirements

| Resource | Minimum |
|----------|---------|
| RAM      | 16 GB   |
| Storage  | 250 GB  |
| CPU      | 4 cores |
| Virtualization Software | VirtualBox (or Azure for Mac M1/M2) |


##  Network Diagram

![soc drawio](https://github.com/user-attachments/assets/58910245-9502-4c54-85d7-239e4e0057e4)

![soc2 drawio](https://github.com/user-attachments/assets/521099ef-db29-4f1a-9f0b-4af39e2b4808)



## 🛠️ Tools & Technologies

- **Sysmon**: Advanced event logging
- **Splunk Universal Forwarder**: Log forwarding agent
- **Atomic Red Team**: Attack simulation framework
- **Windows Server 2019/2022**
- **Windows 10 Enterprise**

##  Resources

- Setup Instructions 
- 
- [SwiftOnSecurity Sysmon Config](https://github.com/SwiftOnSecurity/sysmon-config)
- [Splunk Free Tier](https://www.splunk.com/en_us/download/splunk-enterprise.html)
- [Windows Evaluation ISOs](https://www.microsoft.com/en-us/evalcenter/)


##  Future Improvements

- Add automated alerting via TheHive or Cortex
- Integrate Wazuh as an alternative SIEM
- Build detection-as-code with Sigma rules
- Add Suricata for network-level logging

---

## Author

**[Your Name]**  
Security Analyst | SOC Enthusiast   
 LinkedIn: [your_linkedin_url]  
 GitHub: [your_github_url]

