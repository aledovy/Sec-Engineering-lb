# Wazuh Detection Engineering Lab

## Overview
This repository contains a **detection engineering lab using Wazuh**, designed to simulate a corporate SIEM environment.  
The lab demonstrates **SIEM deployment, endpoint monitoring, log collection, and custom detection rules** using two Ubuntu virtual machines:

- **VM1:** Wazuh Manager, Indexer, Dashboard  
- **VM2:** Wazuh Agent

The goal is to showcase skills in:

- SIEM configuration and management  
- Log analysis and correlation  
- Custom detection rule creation  
- Threat hunting simulations  
- Dashboard visualization  

---

## Architecture

### Lab Architecture
![Lab Architecture](architecture/lab-diagram.png)

- Wazuh Manager receives logs from the Agent  
- Indexer stores and indexes the logs  
- Dashboard visualizes alerts and system activity  
- Network bridge simulates enterprise LAN communication  

### Production vs Lab Comparison
![Production vs Lab](architecture/production-vs-lab.png)

> Note: This lab simulates enterprise deployment but uses VMs instead of physical or cloud servers.

---

## Setup Instructions

### VM-Based Setup

1. **VM1 (Wazuh Server)**
    - Ubuntu 22.04 LTS  
    - Install Wazuh Manager, Indexer, Dashboard
    - Configure firewall and ports (default: 1514, 1515, 5601)  
    - Start services and verify connectivity  

#### 📌 Quickstart Installation (Manager + Indexer + Dashboard)

Wazuh provides an automated installer that deploys all components in one step.  
Run the following command on the Wazuh Server VM:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```


2. **VM2 (Wazuh Agent)**
    - Ubuntu 22.04 LTS  
    - Install Wazuh Agent  
    - Register agent with Manager  
    - Test agent-manager communication 
 
```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.1-1_amd64.deb && sudo WAZUH_MANAGER='192.169.1.8' WAZUH_AGENT_NAME='name_1' dpkg -i ./wazuh-agent_4.14.1-1_amd64.deb
```

Start agent

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

3. **Optional Docker Version**
    - See `setup/docker-setup.md` for containerized deployment  

---

## Configuration

### Wazuh Manager Config
- File: `configs/manager/ossec.conf`  
- Custom rules, decoders, and log collection paths defined here  

### Wazuh Agent Config
- File: `configs/agent/ossec.conf`  
- Configured to forward syslog, authentication logs, and custom events  

---

## Detection Rules

- **Custom Wazuh rules:** `detections/custom_rules.xml`  
- **Sigma rules:** `detections/sigma-rules/`  
- Each rule includes MITRE ATT&CK mapping, description, and detection logic  

### Example Detection
**Detection:** Mimikatz activity via PowerShell  
- Monitors for credential dumping attempts  
- Triggers alert on Wazuh Dashboard  
- See `writeups/detection-mimikatz.md` for details

---

## Testing Detections

- Run provided scripts in `detections/detection-tests/` to generate events  
- Screenshots of alerts are in `detections/detection-tests/screenshots/`  
- Logs are collected in `logs-samples/` for reference  

**Example Test Steps:**
1. Execute `test_mimikatz.ps1` on agent VM  
2. Wazuh Manager raises alert  
3. Verify alert on Dashboard  

---

## Logs Samples

- Linux authentication logs: `logs-samples/linux-auth.log`  
- Windows Sysmon logs: `logs-samples/windows-sysmon.log`  

These logs were sanitized for security and privacy.

---

## Writeups

Detailed detection and threat-hunting notes:

- `writeups/detection-mimikatz.md`  
- `writeups/detection-bruteforce.md`  
- `writeups/threat-hunting-notes.md`  

Each writeup includes:

- Detection description  
- MITRE ATT&CK mapping  
- Testing methodology  
- Dashboard screenshot evidence  

---

## Learnings

- Deployment of Wazuh SIEM in a VM environment  
- Configuration of agent-manager communication and log forwarding  
- Creation and testing of custom detection rules  
- Threat hunting using real-world attack simulations  
- Dashboard visualization and alert validation  

---

## Future Enhancements

- Add **multi-agent lab** with more endpoints  
- Implement **automated detection scripts**  
- Simulate **ransomware and privilege escalation attacks**  
- Create **Docker version** for reproducible labs  

---

## References

- [Wazuh Documentation](https://documentation.wazuh.com/)  
- [Sigma Rules Repository](https://github.com/SigmaHQ/sigma)  
- [MITRE ATT&CK Framework](https://attack.mitre.org/)  

---

*This lab is intended for **educational and portfolio purposes only.** Do not use in production or against unauthorized systems.*
