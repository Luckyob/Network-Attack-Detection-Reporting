# Network Attack Detection & Reporting

### Cowrie SSH Honeypot | Nmap Reconnaissance | Wireshark Traffic Analysis

![Kali Linux](https://img.shields.io/badge/Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-004170?style=for-the-badge)
![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white)
![Cowrie](https://img.shields.io/badge/Cowrie-Honeypot-red?style=for-the-badge)

> **Cybersecurity Class — Assignment 1**
> A controlled network security laboratory demonstrating reconnaissance, honeypot deployment, packet capture, and basic traffic analysis.

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Lab Architecture](#️-lab-architecture)
- [Cowrie Honeypot Deployment](#-cowrie-honeypot-deployment)
- [Network Configuration](#-network-configuration)
- [Nmap Reconnaissance](#-nmap-reconnaissance)
- [Wireshark Traffic Analysis](#-wireshark-traffic-analysis)
- [Cowrie Evidence](#-cowrie-evidence)
- [Key Findings](#-key-findings)
- [Security Recommendations](#️-security-recommendations)
- [Repository Structure](#-repository-structure)
- [Network Capture](#-network-capture)
- [Incident Report](#-incident-report)
- [Learning Outcomes](#-learning-outcomes)
- [Ethical & Safety Notice](#️-ethical--safety-notice)
- [Author](#-author)

---

## 📌 Project Overview

This project demonstrates a controlled network security assessment using **Kali Linux, Ubuntu Server, Cowrie SSH Honeypot, Nmap, and Wireshark**.

The laboratory environment was created using **VirtualBox** with an isolated Host-Only network. Kali Linux served as the testing and reconnaissance machine, while Ubuntu Server hosted the Cowrie SSH honeypot.

The exercise focused on identifying exposed services, generating reconnaissance traffic, capturing the resulting network packets, and analyzing the communication between the testing machine and the honeypot.

### 🎯 Objectives

- Configure an isolated cybersecurity laboratory using VirtualBox.
- Deploy and run Cowrie SSH Honeypot on Ubuntu Server.
- Establish connectivity between Kali Linux and the honeypot.
- Perform service and version reconnaissance using Nmap.
- Capture reconnaissance traffic using Wireshark.
- Analyze TCP connection activity.
- Document findings and recommend defensive measures.

---

## 🏗️ Lab Architecture

| Component         | Role                     | IP Address        |
| ----------------- | ------------------------ | ----------------- |
| Kali Linux        | Testing / Reconnaissance | `192.168.56.101`  |
| Ubuntu Server     | Honeypot Host            | `192.168.56.10`   |
| Cowrie            | SSH Honeypot             | `TCP/222`         |
| VirtualBox        | Virtualization Platform  | —                 |
| Host-Only Network | Isolated Lab Network     | `192.168.56.0/24` |

### Network Flow

```text
┌──────────────────────────────┐
│        Kali Linux            │
│      192.168.56.101          │
│                              │
│     Nmap + Wireshark         │
└──────────────┬───────────────┘
               │
               │ Host-Only Network
               │ 192.168.56.0/24
               │
               ▼
┌──────────────────────────────┐
│       Ubuntu Server          │
│      192.168.56.10           │
│                              │
│      Cowrie Honeypot         │
│         TCP/222              │
└──────────────────────────────┘
```

---

## 🐧 Cowrie Honeypot Deployment

Cowrie was installed on Ubuntu Server inside a Python virtual environment.

### 1. Install Dependencies

```bash
sudo apt update
sudo apt install git python3-venv python3-pip -y
```

### 2. Clone Cowrie

```bash
git clone https://github.com/cowrie/cowrie.git
cd cowrie
```

### 3. Create the Python Virtual Environment

```bash
python3 -m venv cowrie-env
source cowrie-env/bin/activate
```

### 4. Install Requirements

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install -e .
```

### 5. Start Cowrie

```bash
cowrie start
```

Verify the service:

```bash
cowrie status
```

Cowrie was successfully deployed and configured to provide an SSH honeypot service on **TCP port 222**.

---

## 🌐 Network Configuration

The lab used two network interfaces on the Ubuntu Server:

- **NAT:** Internet connectivity and package installation.
- **Host-Only:** Isolated communication with Kali Linux.

| Machine       | Host-Only IP     |
| ------------- | ---------------- |
| Ubuntu Server | `192.168.56.10`  |
| Kali Linux    | `192.168.56.101` |

Connectivity was verified from Kali Linux:

```bash
ping 192.168.56.10
```

Successful ICMP replies confirmed connectivity between the two virtual machines.

---

## 🔎 Nmap Reconnaissance

Nmap was used to perform service and version detection against the Cowrie honeypot.

### Command

```bash
nmap -sV 192.168.56.10
```

### Results

The scan identified the following service:

| Finding                  | Result               |
| ------------------------ | -------------------- |
| Target                   | `192.168.56.10`      |
| Port                     | `222/tcp`            |
| State                    | `open`               |
| Service                  | SSH                  |
| Version Detected         | OpenSSH 9.2p1 Debian |
| Protocol                 | SSH 2.0              |
| Operating System         | Linux                |
| Network Interface Vendor | Oracle VirtualBox    |

Nmap identified the exposed service as OpenSSH. In this laboratory, however, the service was being provided by **Cowrie**, which emulates an SSH environment for security monitoring and research.

### Nmap Evidence

<img width="1366" height="627" alt="test2" src="https://github.com/user-attachments/assets/5e85be2e-01f5-43b7-a9d0-797c448e90f6" />
<img width="1366" height="627" alt="test" src="https://github.com/user-attachments/assets/8d4116a8-808c-4eaa-9cdb-e3f713c4f5c3" />


---

## 📡 Wireshark Traffic Analysis

Wireshark was used to capture the network traffic generated during the Nmap reconnaissance.

The capture was performed on Kali Linux using the Host-Only interface.

### Observed Communication

```text
Source:       192.168.56.101
Destination:  192.168.56.10
Protocol:     TCP
```

The capture contained TCP **SYN** packets generated during the reconnaissance activity and **RST, ACK** responses from the target.

These packets provided evidence of TCP connection attempts between Kali Linux and the Cowrie honeypot.

### Display Filter

The following Wireshark display filter was used to isolate traffic involving the honeypot:

```text
ip.addr == 192.168.56.10
```

### Wireshark Evidence

<img width="988" height="627" alt="test5" src="https://github.com/user-attachments/assets/a64cd9bf-5361-4477-a2e9-bfdb35984743" />
<img width="988" height="627" alt="test3" src="https://github.com/user-attachments/assets/571040d7-b89e-4135-892a-546557805408" />
<img width="988" height="627" alt="test4" src="https://github.com/user-attachments/assets/2a32d19b-4b96-47b6-9e59-b0deee86f268" />




---

## 🍯 Cowrie Evidence

The Cowrie service was verified as running on the Ubuntu Server.

<img width="1358" height="676" alt="Capture" src="https://github.com/user-attachments/assets/437473ad-aa9a-40d8-bed2-7a2cd90e13d5" />
<img width="1365" height="711" alt="Capture24" src="https://github.com/user-attachments/assets/02e9e76f-9d75-40be-b0bd-68248e508a9a" />
<img width="1365" height="711" alt="Capture23" src="https://github.com/user-attachments/assets/bcac214f-a08e-4324-99bb-5cac262921b9" />


| Detail     | Value           |
| ---------- | --------------- |
| Service    | SSH Honeypot    |
| Port       | `TCP/222`       |
| Host       | Ubuntu Server   |
| IP Address | `192.168.56.10` |

---

## 📊 Key Findings

### 1. Exposed SSH Service

Nmap identified **TCP port 222** as open and detected an SSH service.

### 2. Reconnaissance Traffic

Wireshark successfully captured network traffic generated by the Nmap scan.

### 3. TCP Connection Activity

The packet capture contained TCP **SYN** packets and **RST, ACK** responses associated with the scanning activity.

### 4. Honeypot Detection Environment

The activity was performed against a deliberately configured Cowrie honeypot, allowing network reconnaissance to be studied safely.

### 5. Network Monitoring

The exercise demonstrated how packet analysis can provide visibility into reconnaissance activity occurring on a network.

---

## 🛡️ Security Recommendations

The following security measures can help reduce the risk associated with network reconnaissance:

- Restrict unnecessary network ports using firewalls.
- Disable unnecessary services.
- Keep operating systems and applications patched.
- Monitor network traffic for scanning activity.
- Deploy IDS/IPS solutions where appropriate.
- Use strong authentication mechanisms.
- Apply the principle of least privilege.
- Monitor suspicious connection attempts.
- Use honeypots to detect and study malicious activity.
- Apply defense-in-depth security controls.

---

## 📁 Repository Structure

```text
Network-Attack-Detection/
│
├── README.md
│
├── screenshots/
│   ├── cowrie-running.png
│   ├── nmap-scan.png
│   └── wireshark-capture.png
│
└── captures/
    └── cowrie-network-attack-capture.pcap
```

---

## 📦 Network Capture

The network traffic captured during the exercise was saved as:

```text
captures/cowrie-network-attack-capture.pcap
```

The original Wireshark capture was also retained in **PCAPNG format** as a backup.

The capture contains the network traffic generated during the authorized Nmap reconnaissance activity.

---

## 📝 Incident Report

A separate one-page incident report was prepared as part of the assignment.

The report covers:

- Incident overview
- Tools used
- Nmap reconnaissance results
- Wireshark traffic analysis
- Security risks
- Recommended defensive measures
- Conclusion

---

## 🔐 Project Workflow

```text
VirtualBox Lab Setup
        │
        ▼
Kali Linux + Ubuntu Server
        │
        ▼
Cowrie Honeypot Deployment
        │
        ▼
Network Connectivity Test
        │
        ▼
Nmap Service Reconnaissance
        │
        ▼
Wireshark Packet Capture
        │
        ▼
Traffic Analysis
        │
        ▼
Incident Report
        │
        ▼
Security Recommendations
```

---

## 🎓 Learning Outcomes

This project provided practical experience with:

- VirtualBox laboratory configuration
- Kali Linux
- Ubuntu Server
- Cowrie SSH Honeypot
- Nmap service discovery
- Wireshark packet capture
- TCP traffic analysis
- Network reconnaissance
- Basic attack detection
- Incident reporting
- Security recommendations

---

## ⚠️ Ethical & Safety Notice

This project was conducted strictly within an **authorized and isolated cybersecurity laboratory**.

All reconnaissance activity was directed only at the deliberately configured Cowrie honeypot:

```text
192.168.56.10
```

No external or unauthorized systems were targeted.

> **Always obtain explicit authorization before performing security testing, scanning, or reconnaissance against systems you do not own or have permission to assess.**

---

## 👤 Author

**Lucky Samuel**
