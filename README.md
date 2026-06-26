# Adversary Emulation & Security Operations Center (AESOC)

AESOC is a cybersecurity homelab project designed to emulate real-world Security Operations Center (SOC) workflows through centralized monitoring, incident investigation, threat hunting, detection engineering, and adversary simulation.

The project focuses on incident investigation, threat hunting, detection engineering, alert tuning, dashboard development, and MITRE ATT&CK-mapped adversary emulation activities.

---

# Project Origin

While completing The Ohio State University's Cybersecurity Bootcamp, I became increasingly interested in the technologies and infrastructure used to support the program's hands-on security exercises. Beyond completing the labs, I wanted to understand how the underlying systems generated telemetry, collected logs, detected attacks, and enabled analysts to investigate security events.

After completing the program, I set out to build my own Security Operations Center environment from the ground up. The goal was not only to recreate many of the concepts explored during the bootcamp, but also to gain practical experience deploying enterprise security tools, engineering detections, investigating alerts, and documenting the full analyst workflow.

The result is AESOC (Adversary Emulation Security Operations Center), a personal SOC environment designed to emulate real-world security operations through centralized monitoring, incident investigation, detection engineering, adversary simulation, and network security analysis. The environment integrates Windows, Linux, web application, and network telemetry to provide hands-on experience with the technologies and workflows commonly used by Security Operations Center (SOC) analysts.


---

# Core Objectives

- Build and operate a centralized SOC environment
- Simulate adversary behavior in a controlled lab
- Perform end-to-end security investigations
- Develop and tune security detections
- Conduct threat hunting and log analysis activities
- Create custom dashboards for security monitoring and visibility
- Map investigations to the MITRE ATT&CK framework
- Document findings using structured SOC reporting practices

---

# Environment Overview

## Physical Infrastructure

| Device | Purpose |
|--------|----------|
| Beelink SER5 Pro | Proxmox virtualization host |
| Beelink EQi12 | OPNsense firewall and network security gateway |
| NetGear GS108TV3 | VLAN-capable managed switch |

## Virtual Infrastructure

| System | Purpose |
|----------|----------|
| Proxmox VE | Virtualization Platform |
| Windows Server 2022 | Active Directory Domain Controller |
| Windows 10 | User Endpoint |
| Rocky Linux | Linux Endpoint |
| Wazuh | SIEM and Endpoint Monitoring |
| Security Onion | Network Security Monitoring (NSM) |
| DVWA | Vulnerable Web Application for Attack Simulation |
| SO-IDH | Honeypot Used for Intrusion Detection and Alert Generation |

## Security Tooling

- Wazuh
- Security Onion
- OPNsense
- Sysmon
- Auditd
- Suricata
- Zeek

## Security Operations Capabilities

- Endpoint Monitoring
- Network Security Monitoring
- Log Analysis
- Incident Investigation
- Threat Hunting
- Detection Engineering
- Detection Tuning
- Security Dashboard Development
- MITRE ATT&CK Mapping
