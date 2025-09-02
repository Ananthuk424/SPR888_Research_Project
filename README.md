# SPR888_Research_Project
Conducted a research project on integrating honeypots with existing security systems to improve threat detection and increase the likelihood of identifying active security threats.
# SPR888 -  Final Project (Integrated Real-Time Anomaly Detection System)

### Team Members

Ananthu Krishna Vadakkeppatt - avadakkeppatt@myseneca.ca

Ornan Roberts - oroberts1@myseneca.ca

Syed Mujahid Hamid Ali - shamid-ali@myseneca.ca

Yannish Kumar Ballachander Sreedevi - yballachandher-sreed@myseneca.ca

## Project Overview
This project implements an integrated anomaly-driven defense system combining Suricata IDS, Wazuh SIEM, and Artillery Honeypot for real-time threat detection and automated response through honeypot redirection.

## System Architecture
- **Network:** 192.168.100.0/27
- **Components:** Suricata IDS, Wazuh SIEM, Artillery Honeypot
- **Virtualization:** VMware

## Software Requirements
- Ubuntu 20.04/22.04 LTS (for main components)
- Kali Linux (for attacker simulation)
- Windows 10 (for legitimate user simulation)

## Installation Guide
### Set up Network Infrastructure
- Configure Network Segmentation
### Install and Configure Wazuh
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
sudo cp configs/wazuh/local_rules.xml /var/ossec/etc/rules/
sudo cp configs/wazuh/ossec.conf /var/ossec/etc/
sudo systemctl restart wazuh-manager
sudo systemctl status wazuh-manager

```
### Install and Configure Suricata
```bash
sudo apt update
sudo apt install suricata -y
sudo cp configs/suricata/suricata.yaml /etc/suricata/
sudo cp configs/suricata/rules/*.rules /etc/suricata/rules/
sudo suricata-update
sudo systemctl restart suricata
sudo systemctl status suricata
sudo tail -f /var/log/suricata/eve.json
```
### Install and Configure Artillery
```bash
git clone https://github.com/BinaryDefense/artillery.git
cd artillery
sudo python setup.py install
sudo cp configs/artillery/config /var/artillery/
sudo artillery
sudo tail -f /var/artillery/logs/artillery.log
```

## Test the System
Legitimate Traffic Testing
```bash
ssh user@production-server-ip
```
Attack Simulation
```bash
for i in {1..6}; do echo -e "user\nwrongpass\nquit" | FTP production-server-ip; done
hydra -l user -P password.txt ssh://production-server-ip
nmap production-server-ip
```

## Verify Detection and Redirection
1. Check the Wazuh dashboard
2. Verify alerts generation
3. Confirm traffic redirection to the honeypot
4. Check honeypot logs for attacker engagement


## Directory Structure
```bash
Project/
├── README.md
├── configs/
│   ├── wazuh/
│   │   ├── ossec.conf
│   │   └── local_rules.xml
│   ├── suricata/
│   │   ├── suricata.yaml
│   │   └── rules/
│   ├── artillery/
│   │   └── config
│   └── network/
│       └── network-diagram
├── dashboards/
└── vm-configs/
    ├── production-server/
    ├── honeypot-server/
    └── attacker-machine/
```
## Reference to the Main Paper
Muhammad, None Alfarizi, Arief Agus Sukmandhani, and Yulius Denny Prabowo, “Network Anomaly Detection Analysis using Artillery Honeypot and Wazuh SIEM,” Nov. 2023, doi: **https://doi.org/10.1109/icced60214.2023.10425009**
