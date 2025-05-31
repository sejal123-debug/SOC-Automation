# SOC Automation Lab Environment Project

A complete on-premises SOC (Security Operations Center) lab built for DFIR (Digital Forensics & Incident Response), threat detection, and log analysis. This environment includes Active Directory, Windows 10, and Kali Linux machines, with centralized log collection and simulated attacks for detection engineering and investigation.

##  Project Objective

To build a self-contained lab for SOC automation. The goal is to:
- Understand and visualize log flows.
- Simulate real-world attacks.
- Analyze security events.
- Automate detection and response playbooks.

## Lab Components


| Category                    | Component/Setting   | Value/Details                                              |
| --------------------------- | ------------------- | ---------------------------------------------------------- |
| **Cloud Provider**          | Platform            | [DigitalOcean](https://www.digitalocean.com/)              |
|                             | Free Tier           | \$200 credit for 60 days (valid credit card required)      |
| **VMs (Droplets)**          | `wazuh`             | Ubuntu 22.04, 4 GB RAM, 50 GB SSD, Premium Intel           |
|                             | `thehive`           | Ubuntu 22.04, 4 GB RAM, 50 GB SSD, Premium Intel           |
| **Firewall Rules**          | SSH                 | Port 22 — Allow only your public IP                        |
|                             | HTTPS               | Ports 443, 5601, 9200 — Allow only your public IP          |
|                             | General             | Deny all other incoming traffic                            |
| **Access Methods**          | SSH                 | PuTTY / VS Code SSH / DigitalOcean Console                 |
|                             | Web Dashboards      | `https://<Wazuh-IP>` and `https://<TheHive-IP>`            |
| **Wazuh**                   | Role                | SIEM (Security Information and Event Management)           |
|                             | URL                 | [https://wazuh.com](https://wazuh.com)                     |
|                             | Installation        | Curl installer from Wazuh official website                 |
|                             | Default User        | `admin` (password saved during install)                    |
| **TheHive**                 | Role                | SOC Case Management & Automation Platform                  |
|                             | URL                 | [https://thehive-project.org](https://thehive-project.org) |
|                             | Required Components | Java 11, Cassandra, Elasticsearch                          |
| **Dependencies**            | Java                | Required for TheHive                                       |
|                             | Apache Cassandra    | Database for TheHive                                       |
|                             | Elasticsearch       | Data indexing and search for TheHive                       |
| **Security Best Practices** | Password Manager    | Use for all VM logins and service accounts                 |
|                             | Update/Upgrade      | `apt update && apt upgrade` after SSH into each VM         |


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

### Steps

- Setup Instructions
- Configuration


SOC Enthusiast   
 LinkedIn: https://linkedin.com/in/sejalvetkar
 GitHub: https://github.com/sejal123-debug

