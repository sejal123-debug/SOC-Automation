# SOC-Automation

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




##  IP Address Scheme

| Device          | IP Address       |
|-----------------|------------------|
| Splunk Server   | 192.168.10.10    |
| AD Server       | 192.168.10.5     |
| Win10 Target    | DHCP Assigned    |
| Kali Linux      | 192.168.10.250   |

Domain Name: `mydfir.local`  
Network Range: `192.168.10.0/24`

---

## 🛠️ Tools & Technologies

- **Sysmon**: Advanced event logging
- **Splunk Universal Forwarder**: Log forwarding agent
- **Atomic Red Team**: Attack simulation framework
- **Windows Server 2019/2022**
- **Windows 10 Enterprise**
- **Kali Linux**

## Setup Instructions

1. **Install VirtualBox** or cloud VMs
2. **Create and configure AD server**
3. **Join Win10 machine to domain**
4. **Install Splunk Enterprise**
5. **Install and configure Splunk Forwarders**
6. **Deploy Sysmon with SwiftOnSecurity config**
7. **Connect all devices to the same internal network**
8. **Verify log flows into Splunk**
   

##  Resources

- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
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

