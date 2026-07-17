# NIST CSF 2.0 Mapping

This document maps current AESOC lab activities to the NIST Cybersecurity Framework 2.0 functions: Govern, Identify, Protect, Detect, Respond, and Recover.

## NIST CSF 2.0 Function Mapping

| NIST CSF 2.0 Function | AESOC Activity | Evidence in Repository | What Was Produced |
|---|---|---|---|
| Govern | Documented the purpose, scope, methodology, and structure of the AESOC lab | Root README, Lab Architecture, SOC Operations, and investigation methodology | Project documentation, SOC workflow structure, and framework mapping |
| Identify | Documented lab assets, monitored endpoints, telemetry sources, network segments, and security tools | Lab Architecture Diagram, Telemetry Flow Diagram, Environment Overview | Asset and environment understanding through diagrams and documentation |
| Protect | Designed a segmented lab environment using OPNsense, VLANs, Active Directory, endpoint agents, and separated monitoring systems | Architecture documentation, telemetry flow documentation, AD/Wazuh/Security Onion environment | Segmented lab design and controlled monitoring architecture |
| Detect | Collected and analyzed endpoint and network telemetry using Wazuh, Security Onion, Sysmon, Auditd, Suricata, and Zeek | 9 investigations, Wazuh dashboard, Security Onion dashboard | Alert evidence, dashboard visibility, detection validation |
| Detect | Identified a detection gap for Windows discovery commands and created a custom Wazuh detection | Detection Engineering 001 - Windows Discovery Activity | Custom Wazuh rule, detection validation, MITRE ATT&CK mapping |
| Detect | Identified noisy PowerShell Script Policy Test alerts and tuned severity while preserving visibility | Detection Tuning 001 - Windows PowerShell Activity | False-positive analysis, custom tuning rule, validation evidence |
| Respond | Performed alert triage, evidence review, investigation, classification, and MITRE ATT&CK mapping | Investigation case folders | Investigation reports, screenshots, findings, lessons learned |
| Recover | Documented recommendations, lessons learned, validation steps, and detection improvements after investigations | Case conclusions, detection tuning validation, lessons learned sections | Follow-up recommendations and validated tuning improvements |

## Strongest Examples

| AESOC Project | NIST CSF Functions | Why It Maps |
|---|---|---|
| WinRM Lateral Movement Investigation | Identify, Detect, Respond, Recover | Identified source/destination hosts, detected WinRM activity with Wazuh and Security Onion, investigated endpoint/network evidence, and documented recommendations |
| Windows Discovery Activity Detection | Identify, Detect, Govern | Identified a telemetry-to-alerting gap and developed a custom Wazuh detection |
| PowerShell Detection Tuning | Detect, Respond, Govern, Recover | Reviewed repeated high-severity alerts, determined benign behavior, tuned severity, and validated the change |
| SQL Injection Investigation | Identify, Detect, Respond, Recover | Identified the affected web application, reviewed Security Onion evidence, analyzed HTTP request activity, and documented findings |
| Wazuh and Security Onion Dashboards | Identify, Detect, Govern | Improved visibility into alert severity, assets, ATT&CK coverage, and network security events |

## Current Control Gaps Identified

| Area | Gap Identified | Action Taken |
|---|---|---|
| Windows discovery monitoring | Discovery commands were visible in telemetry but did not generate a dedicated detection | Built a custom Wazuh rule for whoami, ipconfig, and systeminfo |
| PowerShell alert quality | Benign PowerShell Script Policy Test activity generated critical-severity alerts | Tuned the alert severity while preserving event visibility |
| Network attack visibility | Web attacks and scan activity required network-level evidence for full context | Used Security Onion, Suricata, and Zeek to validate web and network investigations |
| Endpoint/network correlation | Some activity required both endpoint and network evidence to confirm behavior | Correlated Wazuh endpoint evidence with Security Onion network evidence in cases such as WinRM lateral movement |
| Dashboard visibility | Raw alerts alone were less useful for quick triage and trend review | Created Wazuh and Security Onion dashboards for monitoring and analyst review |

## Summary

AESOC demonstrates practical exposure to NIST CSF 2.0 by mapping completed SOC lab activities to cybersecurity outcomes. The current lab shows asset identification, segmented architecture, endpoint and network monitoring, alert triage, incident investigation, detection engineering, detection tuning, dashboard development, documentation, and control gap analysis.
