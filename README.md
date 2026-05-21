👨‍💻 SOC Home Lab: Attack & Defense Simulation 🚀
Table of Contents
Introduction
Prerequisites
Network Topology
Step 1: Setting Up Virtual Machines
Step 2: Configuring NAT Network
Step 3: Installing Splunk SIEM
Step 4: Installing Sysmon on Windows 11
Step 5: Configuring Splunk Log Ingestion
Step 6: Attack Simulation Using Kali Linux & Atomic Red Team
Step 7: Threat Hunting with Splunk
Detection Queries
Issues Faced & Troubleshooting
Skills Demonstrated
Conclusion


📌 Introduction

This project demonstrates the setup of a complete SOC (Security Operations Center) home lab environment for attack simulation and defensive monitoring.

The lab consists of:

Kali Linux attacker machine
Windows 11 target machine
Splunk SIEM for centralized logging
Sysmon for advanced endpoint telemetry
Atomic Red Team for attack simulation

The objective of this project was to simulate attacker activity, collect logs, detect suspicious behavior, and perform threat hunting using enterprise-style SOC workflows.

🔧 Prerequisites
Requirement	Description
RAM	Recommended 16 GB minimum
Virtualization Software	Oracle VirtualBox
Operating Systems	Windows 11 ISO & Kali Linux ISO
SIEM Platform	Splunk Enterprise
Endpoint Logging	Sysmon
Internet Connection	Required for downloads and configuration
🌐 Network Topology
[Kali Linux (Attacker)]  --->  [Windows 11 VM (Target)]  --->  [Splunk SIEM]
Kali Linux was used for attack simulation.
Windows 11 acted as the victim endpoint.
Splunk collected and analyzed telemetry logs.
🖥️ Step 1: Setting Up Virtual Machines
1.1 Install Kali Linux (Attacker Machine)
Download Kali Linux ISO from the official Kali Linux website.
Create a new VirtualBox VM.
Allocate:
2 GB RAM
2 CPU cores
Install Kali Linux using:
XFCE Desktop Environment
Top10 Tools
Default Recommended Tools

Update Kali Linux:

sudo apt update && sudo apt upgrade -y
1.2 Install Windows 11 (Target Machine)
Download Windows 11 ISO.
Create a Windows VM in VirtualBox.
Allocate:
4 GB RAM
2 CPU cores

During setup:

Used local account setup
Configured networking
Enabled internet connectivity
🌐 Step 2: Configuring NAT Network

A dedicated VirtualBox NAT Network named:

SOC-LAB

was created for communication between VMs.

Both Windows and Kali VMs were connected to:

NAT Network → SOC-LAB

This enabled:

Internal VM communication
Isolated attack traffic
Internet access
📊 Step 3: Installing Splunk SIEM
Splunk Installation
Installed Splunk Enterprise on the Windows 11 VM.
Accessed Splunk Web Interface:
http://localhost:8000
Logged in using administrator credentials.
🛡️ Step 4: Installing Sysmon on Windows 11
Objective

Sysmon was installed to provide enhanced Windows endpoint telemetry including:

Process creation
Network connections
DNS queries
Registry modifications
File creation events
Sysmon Installation

Downloaded:

Sysmon from Microsoft Sysinternals
SwiftOnSecurity Sysmon configuration

Installation command:

Sysmon64.exe -accepteula -i sysmonconfig-export.xml

Verification:

Get-Service Sysmon64

Logs verified from:

Microsoft-Windows-Sysmon/Operational
📥 Step 5: Configuring Splunk Log Ingestion

Created:

inputs.conf

inside:

C:\Program Files\Splunk\etc\system\local
inputs.conf Configuration
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
start_from = oldest
current_only = false
checkpointInterval = 5
renderXml = true
index = main

Restarted Splunk:

splunk stop
splunk start
⚔️ Step 6: Attack Simulation Using Kali Linux & Atomic Red Team
Objective

Kali Linux and Atomic Red Team were used to simulate attacker behavior and generate telemetry for detection in Splunk.

MITRE ATT&CK Techniques Simulated
Technique ID	Description
T1059.001	PowerShell Execution
T1105	Ingress Tool Transfer
T1018	Remote System Discovery
T1090	Proxy Activity
Commands Executed
PowerShell Activity
Get-Process
Network Activity
ping google.com
DNS Queries
nslookup google.com
Web Requests
Invoke-WebRequest https://example.com
🔍 Step 7: Threat Hunting with Splunk

Used Splunk Search & Reporting for telemetry analysis and detection.

Detection Queries
PowerShell Detection
index=main powershell
Process Creation Events
index=main "<EventID>1</EventID>"
Network Connection Events
index=main "<EventID>3</EventID>"
DNS Query Events
index=main "<EventID>22</EventID>"
📌 Key Sysmon Event IDs
Event ID	Description
1	Process Creation
3	Network Connection
11	File Creation
13	Registry Modification
22	DNS Query
⚠️ Issues Faced & Troubleshooting
1. VirtualBox Display Issues
Problem

Kali Linux VM initially showed:

Black screen
Blinking cursor
Boot instability
Solution
Disabled 3D Acceleration
Changed Graphics Controller
Reduced VM RAM allocation
2. Host OS Instability
Problem

Host operating system became unstable and displayed:

Media failure messages
Recovery options
Temporary boot issues
Cause
Running multiple VMs simultaneously
RAM exhaustion
VirtualBox graphics instability
Solution
Reduced VM resources
Optimized VirtualBox display settings
Avoided running both VMs during setup
3. Kali Linux Boot Problems
Problem

Kali Linux initially failed to boot correctly.

Cause
Incorrect boot order
EFI conflicts
ISO mounting issues
Solution
Disabled EFI
Corrected boot order
Installed GRUB bootloader properly
4. Splunk Log Ingestion Issues
Problem

Splunk initially returned:

0 events found
Cause
Incorrect inputs.conf configuration
Splunk restart issues
XML parsing limitations
Solution
Corrected inputs.conf
Restarted Splunk services
Used XML-based search queries
5. Windows File Extension Issue
Problem

Configuration file was accidentally saved as:

inputs.conf.txt

instead of:

inputs.conf
Solution
Enabled file extensions
Renamed configuration file correctly
🧠 Skills Demonstrated
SIEM Technologies
Splunk Enterprise
SPL Query Development
Log Ingestion
Endpoint Monitoring
Sysmon Deployment
Windows Event Logging
Threat Detection
Threat Hunting
Event Correlation
PowerShell Monitoring
Attack Simulation
Kali Linux
Atomic Red Team
MITRE ATT&CK Techniques
Virtualization
Oracle VirtualBox
NAT Networking
VM Troubleshooting
✅ Conclusion

This project successfully implemented a fully functional SOC home lab environment capable of:

Collecting Windows telemetry
Ingesting logs into Splunk SIEM
Simulating attacker activity using Kali Linux and Atomic Red Team
Detecting suspicious behavior
Performing threat hunting and investigation

The project provided practical hands-on experience with enterprise-style SOC operations, SIEM monitoring, attack simulation, and cybersecurity investigation workflows.
