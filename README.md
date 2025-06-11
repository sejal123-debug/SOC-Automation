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
| **Wazuh Manager**           | Ubuntu 20.04+       | Log correlation + alerting                                 |
|                             | Default User        | `admin` (password saved during install)                    |
| **TheHive**                 | Role                | SOC Case Management & Automation Platform                  |
|                             | URL                 | [https://thehive-project.org](https://thehive-project.org) |
|                             | Required Components | Java 11, Cassandra, Elasticsearch                          |
| **Dependencies**            | Java                | Required for TheHive                                       |
|                             | Apache Cassandra    | Database for TheHive                                       |
|                             | Elasticsearch       | Data indexing and search for TheHive                       |
| **Security Best Practices** | Password Manager    | Use for all VM logins and service accounts                 |
|                             | Update/Upgrade      | `apt update && apt upgrade` after SSH into each VM         |
| **Windows 10**              | Windows 10 Pro      | Endpoint + Sysmon + Mimikatz                               |
| **Sysmon**                  | Windows utility     | Process telemetry                                          |

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

- [Setup Instructions](https://github.com/sejal123-debug/SOC-Automation/tree/main/soc_setup)
- [Configuration](https://github.com/sejal123-debug/SOC-Automation/tree/main/Configuration)



# Configuration Guide

configuration of TheHive, Cassandra, Elasticsearch, and Wazuh on a virtual SOC home lab.

* Cassandra and Elasticsearch configured and integrated
* TheHive service up and accessible
* Wazuh Manager and Agent connected and functional
* Mimikatz detection capability validated

## 1. Configure Cassandra (TheHive's Backend Database)

###  Configuration Path

```bash
sudo nano /etc/cassandra/cassandra.yaml
```

###  Changes to Make

* Change the **cluster\_name** to a custom name:

  ```yaml
  cluster_name: 'my-dfir'
  ```
* Update **listen\_address** and **rpc\_address** from `localhost` to your Hive VM’s public IP.
* Update **seed\_provider** to replace `127.0.0.1` with your public IP.

###  Restart and Verify

```bash
sudo systemctl stop cassandra
sudo rm -rf /var/lib/cassandra/*
sudo systemctl start cassandra
sudo systemctl status cassandra
```

---

## 2. Configure Elasticsearch

### Configuration Path

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

###  Changes to Make

* Un-comment and set:

  ```yaml
  cluster.name: thehive
  node.name: node-1
  network.host: <Your_Public_IP>
  http.port: 9200
  cluster.initial_master_nodes: ["node-1"]
  ```

### Start and Enable

```bash
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
sudo systemctl status elasticsearch
```

### Fixing Elasticsearch Memory Issues

If you face issues logging in to TheHive, create a custom memory config:

```bash
sudo nano /etc/elasticsearch/jvm.options.d/jvm.options
```

Paste:

```bash
-Xms2g
-Xmx2g
```

Then restart Elasticsearch:

```bash
sudo systemctl restart elasticsearch
```
---

## 3. Configure TheHive

###  Permissions

Make sure TheHive has write access:
  check using 
  ```bash
ls -la /opt/thp
```
  chage if thehive is in root directory using:
```bash
sudo chown -R thehive:thehive /opt/thp
```

###  Config Path

```bash
sudo nano /etc/thehive/application.conf
```

###  Changes to Make

* Update database host:

  ```hocon
  db {
    host = "<Your_Public_IP>"  public ip of thehive
    cluster-name = "my-dfir"
  }
  ```
* Update `index` section to use your Elasticsearch public IP.
* Ensure the `storage.path` belongs to TheHive user.
* Change:

  ```hocon
  play.http.context = "http://<Your_Public_IP>:9000/"
  ```

### Start and Enable

```bash
sudo systemctl start thehive
sudo systemctl enable thehive
sudo systemctl status thehive
```
---

##  4. Set Up Wazuh Manager & Agent

###  Find Admin Credentials

```bash
tar -xvf wazuh-install-files.tar
cd wazuh-install-files
cat wazuh-passwords.txt
```

Save the `admin` and `api_user` passwords for login and integrations.

###  Access Wazuh Dashboard

Log in to `https://<Wazuh_IP>` using admin credentials.

> You’ll see **"No agents connected"** – let’s add one!

---

##  5. Connect Windows Agent

###  Add Agent via Dashboard

* Go to `Agents > Add agent`
* Choose **Windows**, input:

  * Manager IP: `Wazuh_Public_IP`
  * Agent Name: `my-dfir`
* Copy the installation command

###  Run on Windows

Open **PowerShell as Administrator**:

```powershell
# Paste the installation script copied from Wazuh
```
Start the service:

```powershell
net start wazuhsvc
```
Or go to `services.msc` and start "Wazuh Service".

 After a few moments, your agent should appear as “Active”.

  (OR)
  
* Run this command on Wazuh console
  bash
  ```
  /var/ossec/bin/manage_agents
  ```
* select A (Add agent) -> give name of agent and public ip address of wazuh -> confirm adding it -> note the agent ID
* select E (Extract key) -> give the agent ID -> note the extracted key
  
* Run this command on power shell
  bash
  ```
   & "C:\Program Files (x86)\ossec-agent\manage_agents.exe"
  ```
* Select Insert -> paste the extracted key here -> Confirm adding it
  
  ---
  
##  Access the Dashboards

* **TheHive**: `http://<Hive_IP>:9000`

  * Default creds: `admin@thehive.local / secret`
* **Wazuh**: `https://<Wazuh_IP>`

---


#  Sysmon Log Ingestion and Mimikatz Detection Using Wazuh

Configuring Windows 10 endpoint to forward Sysmon logs to a Wazuh manager, detect Mimikatz execution (even if renamed), and visualize events via the Wazuh dashboard. The setup emphasizes endpoint telemetry, rule customization, and real-time alerting as part of a **SOC automation/detection lab**.

---

## Project Structure

```
/wazuh-mimikatz-detection/
├── Windows10/
│   └── Sysmon installation & ossec.conf customization
├── Wazuh-Manager/
│   ├── Configuration of ossec.conf for log ingestion
│   └── Custom rule creation
├── Filebeat/
│   └── Archive ingestion setup
└── Docs/
    └── Detection logic, screenshots, and references
```

---

##  Step 1: Configure Wazuh Agent on Windows 10

### Location of Wazuh agent configuration:

```
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Make a **backup** of `ossec.conf` before modifying.

### Update Log Collection Section

In `ossec.conf`, modify the `<localfile>` section to ingest Sysmon logs:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

You can find the correct event log channel by going to:

```
Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational → Properties
```

**Important**: Remove default `<localfile>` entries (e.g., `Application`, `Security`, `System`) if you're focusing solely on Sysmon.

---

##  Step 2: Restart Wazuh Agent Service

```bash
# On Windows (Run as Administrator):
services.msc → Restart Wazuh Agent
```

Or via CLI:

```powershell
Restart-Service -Name wazuh
```

---

##  Step 3: Configure Wazuh Manager to Archive All Events

### Location:

```
/var/ossec/etc/ossec.conf
```

Set the following fields to `yes`:

```xml
<logall_json>yes</logall_json>
<logall>yes</logall>
```

>  Restart the Wazuh manager to apply changes:

```bash
sudo systemctl restart wazuh-manager
```

---

##  Step 4: Enable Filebeat Archive Ingestion

Edit the Filebeat configuration file:

```bash
sudo nano /etc/filebeat/filebeat.yml
```

Find the archive ingestion section and set:

```yaml
archives:
  enabled: true
```

Then restart Filebeat:

```bash
sudo systemctl restart filebeat
```

---

##  Step 5: Create a New Index in Wazuh Dashboard

1. Navigate to **Stack Management → Index Patterns**
2. Click **Create index pattern**
3. Name it: `wazuh-archives-*`
4. Select `@timestamp` as the time field
5. Save

This lets you query archived logs in **Discover** view.

---

##  Step 6: Simulate Attack — Download & Run Mimikatz

>  This step is for educational labs only. Do not perform this on a production system.

1. Exclude your Downloads folder from Windows Defender
2. Disable browser protection if Mimikatz is blocked
3. Extract and run `mimikatz.exe` from PowerShell:

```powershell
cd Downloads\mimikatz
.\mimikatz.exe
```

4. You can also **rename** the executable:

```powershell
Rename-Item .\mimikatz.exe .\you_are_awesome.exe
.\you_are_awesome.exe
```

---

##  Step 7: Create a Custom Wazuh Rule to Detect Mimikatz

In Wazuh Dashboard:

1. Go to **Rules → Manage Rule Files**
2. Open `local_rules.xml`
3. Paste the following rule:

```xml
<rule id="100002" level="15">
  <if_sid>80001</if_sid>
  <field name="OriginalFileName">(?i)mimikatz</field>
  <description>Mimikatz use detected (via OriginalFileName)</description>
  <mitre>
    <id>T1003</id>
  </mitre>
</rule>
```

4. Save and restart the Wazuh Manager.

> We use the `OriginalFileName` field to detect even **renamed** binaries.

---

##  Step 8: Verify Detection

In **Discover → Archives Index**:

* Search: `mimikatz`
* You should see Event ID 1 (process creation) with:

  * `OriginalFileName: Mimikatz`
  * `Image: C:\Users\...\you_are_awesome.exe`

You should also see an **alert** under Security Events if the rule triggered successfully.

---

##  Dashboard Screenshots


##  Key Learnings

* Wazuh by default only logs **alerts**, not raw events — unless configured.
* You must enable `logall` and archive features to capture full telemetry.
* **OriginalFileName** is a robust detection method to evade name obfuscation.
* Custom rules help in building targeted detections aligned with MITRE ATT\&CK.

---

##  Future Enhancements

* Automate rule deployment using Ansible or scripts
* Extend detection to cover other credential dumping tools
* Integrate with TheHive or a ticketing system for alert triage
* Visualize attack timeline with Kibana Dashboards

---

##  References

* [Wazuh Documentation](https://documentation.wazuh.com/)
* [Sysmon Event IDs](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
* [Mimikatz GitHub](https://github.com/gentilkiwi/mimikatz)
* [MITRE ATT\&CK - T1003](https://attack.mitre.org/techniques/T1003/)

---

SOC Enthusiast   
 LinkedIn: https://linkedin.com/in/sejalvetkar
 GitHub: https://github.com/sejal123-debug

