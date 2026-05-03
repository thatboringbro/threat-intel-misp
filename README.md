# Nexus Zeta: Threat Intelligence & Actor Profiling

---

## Executive Summary
This project documents a comprehensive investigation into **Nexus Zeta**, a threat actor specializing in large-scale IoT device takeovers. My analysis identified active infrastructure located in the **Netherlands** communicating with thousands of infected devices via a sophisticated version of the **"Masuta"** malware. The actor targets vulnerabilities in common router software, specifically the **D-Link HNAP/SOAP** protocol, to infect devices in seconds.

---

## Technical Breakdown & Intel Gathering

### 1. MISP Integration
For this investigation I utilized an **MISP (Malware Information Sharing Platform)** instance deployed via **Docker** to collect, store, and correlate threat intelligence.
* **Data Enrichment**: Built-in feeds were enabled to cache metadata and extract indicators using UUIDs from Tenable feeds (CIRCL, etc).
* **Correlation**: MISP Event 1316 directly correlates Nexus Zeta IOCs with established Mirai variants such as **Satori** and **PureMasuta**.

### 2. Infrastructure Summary
| Type | Value | Purpose | Confidence |
| :--- | :--- | :--- | :--- |
| **C2 Server IP** | `93.174.93.63` | Botnet Command & Control  | High  |
| **C2 Domain** | `nexusiotsolutions.net` | Primary C2 Domain  | High  |
| **Email** | `nexuszeta1337@gmail.com` | Domain Registration Contact  | High  |

### 3. Threat Actor Profile
* **Identity**: Kenneth Currin Schuchman (aka Nexus Zeta).
* **Origin**: Vancouver, Washington, USA.
* **Motivation**: Financial gain through DDoS-for-hire and cryptocurrency mining.
* **Evolution**: Transitioned from Satori (Mirai-based) to Masuta, shifting from Telnet brute-forcing to exploit-based infection.
* **Skill Level**: Script-kiddie (Amateur) due to heavy reliance on leaked Mirai source-code.

---

## MITRE ATT&CK Mapping
| Tactic | Technique | ID | Application |
| :--- | :--- | :--- | :--- |
| **Initial Access** | Exploit Public-Facing Application | **T1190** | Targeted **CVE-2017-17215** (Huawei) and **CVE-2014-8361** (Realtek). |
| **Execution** | Exploitation for Client Execution | **T1203** | Malicious SOAP commands triggered remote code execution on router processors. |
| **Persistence** | Boot/Logon Initialization Scripts | **T1037** | Malware embedded in device memory as `/bin/busybox MASUTA`. |
| **Impact** | Network Denial of Service | **T1498** | Core DDoS-for-hire service using UDP/TCP flood commands. |

---

## Recommended Mitigations

### Network-Level (Immediate)
* **Blocklist**: Block primary C2 IPs (`93.174.93.63`, `185.244.25.162`) and the `nexusiotsolutions.net` domain at the perimeter firewall.
* **Egress Filtering**: Block outbound connections to port **8080** to disrupt C2 communication.

### Device-Level (IoT/Routers)
* **Service Management**: Disable unnecessary SOAP services and Telnet.
* **Firmware**: Apply regular updates to patch **CVE-2017-17215** and **CVE-2018-1000049**.

### IMAGES
Screenshots for this project can be found in the `/images` folder.

### CONTACT:
X (Twitter): https://x.com/thatboringbro
