# Adversary Emulation & Security Operations Center (AESOC)

![Lab Architecture](Documentation/01-Lab-Architecture.png)

## Portfolio Highlights

* 9 Security Investigations
* 1 Detection Engineering Project
* 1 Detection Tuning Project
* 2 SOC Monitoring Dashboards
* Windows and Linux Telemetry Analysis
* Wazuh SIEM and Security Onion NSM
* MITRE ATT&CK Mapped Investigations
* NIST CSF 2.0 and GRC Framework Mapping

## Best SOC Work

| Type                     | Case Study                                                                                                            | What It Demonstrates                                                                                                                                 |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Detection Engineering    | [Windows Discovery Activity Detection](./Detection-Engineering/Detection-Engineering-001-Windows-Discovery-Activity/) | Identifying a detection gap, validating telemetry, developing a custom Wazuh detection rule, and validating alert generation through testing         |
| Incident Investigation   | [WinRM Lateral Movement Investigation](./Investigations/Case-003-WinRM-Lateral-Movement/)                             | Correlating Wazuh endpoint telemetry with Security Onion network evidence during a Windows Remote Management (WinRM) lateral movement investigation  |
| Web Attack Investigation | [SQL Injection Investigation](./Investigations/Case-007-SQL-Injection-Investigation/)                                 | Investigating a web application attack using Security Onion, HTTP request analysis, source attribution, and documented SOC investigation methodology |

These case studies demonstrate alert triage, detection validation, incident investigation, MITRE ATT&CK mapping, endpoint and network telemetry correlation, detection engineering, and professional SOC documentation practices.

## Technologies Used

* Wazuh
* Security Onion
* Sysmon
* Auditd
* Suricata
* Zeek
* Windows Server 2022
* Windows 10
* Rocky Linux
* OPNsense
* Proxmox VE

## Skills Demonstrated

* Security Monitoring
* Incident Investigation
* Threat Hunting
* Detection Engineering
* Detection Tuning
* Network Security Monitoring
* SIEM Analysis
* Log Analysis
* MITRE ATT&CK Mapping
* Dashboard Development

## Cybersecurity Frameworks

AESOC includes framework mapping to connect hands-on SOC lab work to security operations, control validation, incident response, detection engineering, and risk management concepts.

| Framework / Concept    | How It Is Used                                                                                                                                                           |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NIST CSF 2.0           | Mapped current lab activities to Govern, Identify, Protect, Detect, Respond, and Recover functions                                                                       |
| MITRE ATT&CK           | Mapped observed adversary behaviors to tactics and techniques across investigations                                                                                      |
| OWASP                  | Applied web application security concepts to SQL injection and malicious file upload investigations                                                                      |
| ISO/IEC 27001 Concepts | Mapped current lab activities to foundational concepts such as logging, monitoring, access control, incident management, evidence collection, and continuous improvement |
| Control Gap Analysis   | Identified detection and monitoring gaps based on available telemetry and investigation evidence                                                                         |

Detailed mappings are available in the [Framework Mapping](Framework-Mapping/) directory.

## Repository Navigation

| Section                                         | Description                                                                                             |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [Documentation](Documentation/)                 | Lab architecture, telemetry flow, attack simulation flow, and incident response workflow diagrams       |
| [Investigations](Investigations/)               | Security investigations involving Windows, Linux, web application, authentication, and network activity |
| [Detection Engineering](Detection-Engineering/) | Custom detection engineering and detection tuning projects                                              |
| [Dashboards](Dashboards/)                       | Wazuh and Security Onion monitoring dashboards                                                          |
| [Framework Mapping](Framework-Mapping/)         | NIST CSF 2.0, MITRE ATT&CK, OWASP, ISO/IEC 27001 concepts, and control gap analysis mapping             |

---

AESOC is a cybersecurity homelab project designed to emulate real-world Security Operations Center (SOC) workflows through centralized monitoring, incident investigation, threat hunting, detection engineering, and adversary simulation.

The project focuses on incident investigation, threat hunting, detection engineering, alert tuning, dashboard development, framework mapping, and MITRE ATT&CK-mapped adversary emulation activities.

---

## Project Origin

While completing The Ohio State University's Cybersecurity Bootcamp, I became increasingly interested in the technologies and infrastructure used to support the program's hands-on security exercises. Beyond completing the labs, I wanted to understand how the underlying systems generated telemetry, collected logs, detected attacks, and enabled analysts to investigate security events.

After completing the program, I set out to build my own Security Operations Center environment from the ground up. The goal was not only to recreate many of the concepts explored during the bootcamp, but also to gain practical experience deploying enterprise security tools, engineering detections, investigating alerts, and documenting the full analyst workflow.

The result is AESOC (Adversary Emulation & Security Operations Center), a personal SOC environment designed to emulate real-world security operations through centralized monitoring, incident investigation, detection engineering, adversary simulation, and network security analysis. The environment integrates Windows, Linux, web application, and network telemetry to provide hands-on experience with the technologies and workflows commonly used by Security Operations Center (SOC) analysts.

---

## Core Objectives

* Build and operate a centralized SOC environment
* Simulate adversary behavior in a controlled lab
* Perform end-to-end security investigations
* Validate and improve detection coverage through custom detection engineering and tuning
* Conduct threat hunting and log analysis activities
* Create custom dashboards for security monitoring and visibility
* Map investigations to the MITRE ATT&CK framework
* Map current lab activities to cybersecurity frameworks and control concepts
* Document findings using structured SOC reporting practices

---

## Environment Overview

### Physical Infrastructure

| Device           | Purpose                                        |
| ---------------- | ---------------------------------------------- |
| Beelink SER5 Pro | Proxmox virtualization host                    |
| Beelink EQi12    | OPNsense firewall and network security gateway |
| Netgear GS108TV3 | VLAN-capable managed switch                    |

### Virtual Infrastructure

| System              | Purpose                                                    |
| ------------------- | ---------------------------------------------------------- |
| Proxmox VE          | Virtualization platform                                    |
| Windows Server 2022 | Active Directory Domain Controller                         |
| Windows 10          | User endpoint                                              |
| Rocky Linux         | Linux endpoint                                             |
| Wazuh               | SIEM and endpoint monitoring                               |
| Security Onion      | Network Security Monitoring (NSM)                          |
| DVWA                | Vulnerable web application for attack simulation           |
| SO-IDH              | Honeypot used for intrusion detection and alert generation |

### Security Tooling

* Wazuh
* Security Onion
* OPNsense
* Sysmon
* Auditd
* Suricata
* Zeek

### Security Operations Capabilities

* Endpoint Monitoring
* Network Security Monitoring
* Log Analysis
* Incident Investigation
* Threat Hunting
* Detection Engineering
* Detection Tuning
* Security Dashboard Development
* MITRE ATT&CK Mapping
* NIST CSF 2.0 Mapping
* Control Gap Analysis
