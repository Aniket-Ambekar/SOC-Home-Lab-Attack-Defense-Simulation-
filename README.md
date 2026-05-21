# 👨‍💻 SOC Home Lab: Attack & Defense Simulation 🚀

> Building a practical SOC (Security Operations Center) home lab using **Splunk**, **Sysmon**, **Windows 11**, **Kali Linux**, and **Atomic Red Team** for attack simulation, threat detection, and log analysis.

---

# 📖 Table of Contents

- [Introduction](#-introduction)
- [Lab Architecture](#-lab-architecture)
- [Technologies Used](#-technologies-used)
- [Lab Setup](#-lab-setup)
- [Step 1: Setting Up Virtual Machines](#-step-1-setting-up-virtual-machines)
- [Step 2: Configuring NAT Networking](#-step-2-configuring-nat-networking)
- [Step 3: Installing Splunk SIEM](#-step-3-installing-splunk-siem)
- [Step 4: Installing Sysmon](#-step-4-installing-sysmon)
- [Step 5: Configuring Splunk Log Ingestion](#-step-5-configuring-splunk-log-ingestion)
- [Step 6: Attack Simulation Using Atomic Red Team](#-step-6-attack-simulation-using-atomic-red-team)
- [Step 7: Threat Hunting with Splunk](#-step-7-threat-hunting-with-splunk)
- [Detection Queries](#-detection-queries)
- [Issues Faced & Troubleshooting](#-issues-faced--troubleshooting)
- [Skills Demonstrated](#-skills-demonstrated)
- [Conclusion](#-conclusion)

---

# 📌 Introduction

This project demonstrates the setup of a fully functional SOC home lab environment designed for:
- Attack simulation
- Endpoint monitoring
- SIEM log ingestion
- Threat detection
- Security investigation

The lab simulates real-world SOC workflows by combining:
- Windows endpoint telemetry
- Offensive security testing
- Splunk SIEM monitoring
- Atomic Red Team attack simulation

---

# 🏗️ Lab Architecture

```text
[Kali Linux Attacker]
          ↓
[Windows 11 Target + Sysmon]
          ↓
[Splunk SIEM Monitoring]
```

---

# 🛠️ Technologies Used

| Tool | Purpose |
|---|---|
| Oracle VirtualBox | Virtualization Platform |
| Windows 11 | Target Endpoint |
| Kali Linux | Attack Simulation |
| Sysmon | Advanced Endpoint Logging |
| Splunk Enterprise | SIEM Platform |
| Atomic Red Team | Attack Simulation Framework |

---

# ⚙️ Lab Setup

| Component | Configuration |
|---|---|
| Windows VM | 4 GB RAM / 2 CPUs |
| Kali Linux VM | 2 GB RAM / 2 CPUs |
| Network Type | NAT Network |
| NAT Network Name | SOC-LAB |

---

# 🖥️ Step 1: Setting Up Virtual Machines

## 🔹 Kali Linux Setup

- Downloaded Kali Linux ISO
- Installed using VirtualBox
- Selected:
  - XFCE Desktop
  - Top10 Tools
  - Default Recommended Tools

### Update Kali

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🔹 Windows 11 Setup

- Installed Windows 11 VM
- Configured local administrator account
- Enabled networking and internet access

---

# 🌐 Step 2: Configuring NAT Networking

Created a dedicated NAT Network in VirtualBox:

```text
SOC-LAB
```

Both VMs were connected to:

```text
NAT Network → SOC-LAB
```

This enabled:
- Internal communication between VMs
- Isolated lab traffic
- Internet access

---

# 📊 Step 3: Installing Splunk SIEM

Installed Splunk Enterprise on the Windows 11 VM.

### Access Splunk Web Interface

```text
http://localhost:8000
```

---

# 🛡️ Step 4: Installing Sysmon

## 📌 Objective

Sysmon was installed to provide advanced Windows telemetry including:
- Process creation logging
- Network connections
- DNS queries
- Registry modifications
- File creation events

---

## 🔹 Sysmon Installation

Downloaded:
- Sysmon from Microsoft Sysinternals
- SwiftOnSecurity Sysmon configuration

### Installation Command

```powershell
Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

### Verification

```powershell
Get-Service Sysmon64
```

---

# 📥 Step 5: Configuring Splunk Log Ingestion

Created:

```text
inputs.conf
```

inside:

```text
C:\Program Files\Splunk\etc\system\local
```

---

## 🔹 inputs.conf Configuration

```ini
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
start_from = oldest
current_only = false
checkpointInterval = 5
renderXml = true
index = main
```

---

## 🔹 Restart Splunk

```cmd
splunk stop
splunk start
```

---

# ⚔️ Step 6: Attack Simulation Using Atomic Red Team

## 📌 Objective

Atomic Red Team and Kali Linux were used to simulate attacker activity and generate telemetry for detection in Splunk.

---

# 🧨 MITRE ATT&CK Techniques Simulated

| Technique ID | Description |
|---|---|
| T1059.001 | PowerShell Execution |
| T1105 | Ingress Tool Transfer |
| T1018 | Remote System Discovery |
| T1090 | Proxy Activity |

---

# 🔥 Commands Executed

## PowerShell Activity

```powershell
Get-Process
```

---

## Network Activity

```powershell
ping google.com
```

---

## DNS Queries

```powershell
nslookup google.com
```

---

## Web Requests

```powershell
Invoke-WebRequest https://example.com
```

---

# 🔍 Step 7: Threat Hunting with Splunk

Used Splunk Search & Reporting to analyze telemetry logs and identify suspicious behavior.

---

# 🧠 Detection Queries

## 🔹 PowerShell Detection

```spl
index=main powershell
```

---

## 🔹 Process Creation Events

```spl
index=main "<EventID>1</EventID>"
```

---

## 🔹 Network Connection Events

```spl
index=main "<EventID>3</EventID>"
```

---

## 🔹 DNS Query Events

```spl
index=main "<EventID>22</EventID>"
```

---

# 📌 Key Sysmon Event IDs

| Event ID | Description |
|---|---|
| 1 | Process Creation |
| 3 | Network Connection |
| 11 | File Creation |
| 13 | Registry Modification |
| 22 | DNS Query |

---

# ⚠️ Issues Faced & Troubleshooting

# 1️⃣ VirtualBox Display Issues

## ❌ Problem
Kali Linux VM showed:
- Black screen
- Blinking cursor
- Boot instability

## ✅ Solution
- Disabled 3D Acceleration
- Changed Graphics Controller
- Reduced VM resource allocation

---

# 2️⃣ Host OS Instability

## ❌ Problem
Host system displayed:
- Media failure messages
- Recovery screen prompts
- Temporary boot issues

## ✅ Cause
- Running multiple VMs simultaneously
- RAM exhaustion
- VirtualBox graphics instability

## ✅ Solution
- Reduced VM RAM
- Optimized VirtualBox display settings
- Avoided running multiple VMs during setup

---

# 3️⃣ Kali Linux Boot Issues

## ❌ Problem
Kali Linux failed to boot correctly.

## ✅ Solution
- Disabled EFI
- Corrected boot order
- Verified ISO attachment
- Installed GRUB properly

---

# 4️⃣ Splunk Log Ingestion Issues

## ❌ Problem

Splunk initially returned:

```text
0 events found
```

## ✅ Cause
- Incorrect inputs.conf configuration
- Splunk restart issues
- XML parsing limitations

## ✅ Solution
- Corrected inputs.conf
- Restarted Splunk services
- Used XML-based search queries

---

# 5️⃣ Windows File Extension Issue

## ❌ Problem

Configuration file saved as:

```text
inputs.conf.txt
```

instead of:

```text
inputs.conf
```

## ✅ Solution
- Enabled file extensions
- Renamed configuration file correctly

---

# 🧠 Skills Demonstrated

## 🔹 SIEM Technologies
- Splunk Enterprise
- SPL Query Development
- Log Ingestion

## 🔹 Endpoint Monitoring
- Sysmon Deployment
- Windows Event Logging

## 🔹 Threat Detection
- Threat Hunting
- Event Correlation
- PowerShell Monitoring

## 🔹 Attack Simulation
- Atomic Red Team
- MITRE ATT&CK Techniques
- Offensive Security Testing

## 🔹 Virtualization
- Oracle VirtualBox
- NAT Networking
- VM Troubleshooting

---

# ✅ Conclusion

This project successfully implemented a complete SOC home lab capable of:
- Collecting Windows telemetry
- Ingesting logs into Splunk SIEM
- Simulating attacker activity using Kali Linux and Atomic Red Team
- Detecting suspicious behavior
- Performing threat hunting and investigation

The project provided practical hands-on experience with:
- SIEM operations
- Endpoint monitoring
- Attack simulation
- SOC workflows
- Threat detection engineering
