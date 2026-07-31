# Adversary Emulation & Security Operations Center (AESOC)

AESOC is a portable, segmented cybersecurity home lab built to demonstrate practical entry-level SOC analyst skills.

The core environment combines endpoint and network monitoring, alert triage, investigation, detection engineering, SOAR automation, case management, ticketing, and controlled adversary simulation. The repository also includes separate Microsoft Azure identity and access management (IAM) investigations covering Azure RBAC, Microsoft Entra sign-in analysis, account containment, and access recovery.

[![AESOC Lab Architecture](01-Lab-Architecture/AESOC-Architecture.png)](01-Lab-Architecture/AESOC-Architecture.png)

[Open the architecture diagram at full size](01-Lab-Architecture/AESOC-Architecture.png)

---

## Portfolio Overview

| Area                    | Current implementation                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------- |
| Security investigations | 12 documented investigations: 9 pre-SOAR, 1 completed full-lifecycle, and 2 Azure IAM investigations            |
| SOAR automation         | 3 validated Shuffle playbooks                                                                                   |
| SOC operations          | Alert-to-resolution workflow, Tier 1 and Tier 2 runbooks, handoffs, and closure checklist                       |
| Detection engineering   | 1 custom Wazuh detection, 1 validated tuning project, and 1 deferred tuning review                              |
| Security monitoring     | 1 Wazuh dashboard, 1 Security Onion dashboard, Azure Activity Logs, and Microsoft Entra sign-in and audit logs  |
| Case management         | TheHive alert and case lifecycle                                                                                |
| Ticketing               | Zammad Detection Engineering and Infrastructure Remediation workflows                                           |
| Notifications           | Slack alert, case, and ticket channels                                                                          |
| Cloud identity security | 2 evidence-backed Azure IAM investigations covering excessive privileges and suspicious authentication activity |
| Framework mapping       | MITRE ATT&CK, NIST CSF 2.0, OWASP, ISO/IEC 27001 concepts, and control-gap analysis                             |

> **Project scope:** AESOC and the Azure IAM exercises are controlled lab environments. Cases, tickets, comments, role assignments, sign-ins, account actions, and notification messages were generated for authorized testing and do not represent production incidents. The Azure investigations are documented as a separate cloud-security workstream and are not presented as integrated with the on-premises Shuffle workflow.

---

## Alert-to-Resolution Workflow

```text
Endpoint Activity
        ↓
Wazuh Detection
        ↓
Shuffle Alert Intake
        ↓
TheHive Alert + Slack Notification
        ↓
Tier 1 Alert Triage
   ┌────┴───────────────┐
   ↓                    ↓
Close Alert       Escalate to Case
                         ↓
                Tier 2 Investigation
             ┌───────────┼────────────┐
             ↓           ↓            ↓
       Direct SOC    Detection    Infrastructure
         Action       Review       Remediation
             ↓           ↓            ↓
         Validate     Zammad        Zammad
          Result       Ticket         Ticket
                          └────┬───────┘
                               ↓
                    Ticket Closure Handback
                               ↓
                       TheHive Case Update
                               ↓
                        Tier 2 Validation
                               ↓
                         Final Case Closure
```

TheHive is the primary alert and investigation record. Zammad tracks work assigned to Detection Engineering or Infrastructure Remediation. Slack provides lifecycle visibility but is not treated as the official investigation record.

Security Onion provides a separate network-monitoring path. Its alerts are currently reviewed within Security Onion and are not yet forwarded into the documented Shuffle automation.

---

## Featured Work

| Project                                                                                                                                      | What it demonstrates                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| [SOC Operations](02-SOC-Operations/)                                                                                                         | Tier 1 triage, Tier 2 investigation, response paths, ownership, handoffs, validation, and case closure                                             |
| [OneDrive-to-Explorer Full-Lifecycle Investigation](04-Investigations/02-Full-Lifecycle-Investigations/01-OneDrive-Explorer-Process-Access/) | Wazuh alert intake, Tier 1 and Tier 2 analysis, Detection Engineering handoff, controlled rule testing, ticket closure, and final case disposition |
| [Wazuh Alert Intake](03-SOAR-Automation/01-Wazuh-Alert-Intake/)                                                                              | Wazuh webhook ingestion, severity routing, TheHive alert creation, and Slack notification                                                          |
| [Case Updates and Ticket Routing](03-SOAR-Automation/02-Case-Updates-and-Ticket-Routing/)                                                    | TheHive lifecycle events, tag-based routing, Zammad ticket creation, and duplicate prevention                                                      |
| [Ticket Closure Handback](03-SOAR-Automation/03-Ticket-Closure-Handback/)                                                                    | Zammad closure processing, TheHive synchronization, and Tier 2 handback                                                                            |
| [WinRM Lateral Movement](04-Investigations/01-Pre-SOAR-Investigations/Case-003-WinRM-Lateral-Movement/)                                      | Correlation of Wazuh endpoint telemetry with Security Onion network evidence                                                                       |
| [Windows Discovery Custom Detection](05-Detection-Engineering/Custom-Detections/Custom-Detection-001-Windows-Discovery-Activity/)            | Detection-gap analysis, Wazuh rule development, testing, and validation                                                                            |
| [PowerShell Detection Tuning](05-Detection-Engineering/Detection-Tuning/Detection-Tuning-001-Windows-PowerShell-Activity/)                   | False-positive analysis, severity tuning, and post-change validation                                                                               |
| [Wazuh Security Monitoring Dashboard](06-SOC-Dashboards/Wazuh/Dashboard-001-Security-Monitoring/)                                            | Endpoint alert and security-event visualization                                                                                                    |
| [Security Onion Network Threat Dashboard](06-SOC-Dashboards/Security-Onion/Dashboard-001-Network-Threat-Monitoring/)                         | Network alert, protocol, and connection visibility                                                                                                 |
| [Unauthorized Azure Contributor Assignment](09-Cloud-Security/01-Azure-IAM/01-Unauthorized-Contributor-Assignment/)                          | Azure RBAC analysis, excessive-permission validation, Activity Log attribution, containment, and least-privilege restoration                       |
| [Suspicious Entra Sign-In and Account Containment](09-Cloud-Security/01-Azure-IAM/02-Suspicious-Sign-In-Account-Containment/)                | Sign-in log analysis, MFA validation, account disablement, session revocation, password reset, and secure recovery                                 |

---

## Repository Navigation

| Section                                                                                 | Contents                                                                                        |
| --------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [01 – Lab Architecture](01-Lab-Architecture/)                                           | Physical and logical architecture, VLANs, telemetry flow, assets, and security stack            |
| [02 – SOC Operations](02-SOC-Operations/)                                               | Alert lifecycle, ownership, Tier 1 and Tier 2 runbooks, and closure procedures                  |
| [03 – SOAR Automation](03-SOAR-Automation/)                                             | Three validated Shuffle workflows with implementation evidence                                  |
| [04 – Investigations](04-Investigations/)                                               | Nine pre-SOAR investigations and one completed full alert-to-resolution lifecycle investigation |
| [05 – Detection Engineering](05-Detection-Engineering/)                                 | Custom detections, detection tuning, testing, and validation                                    |
| [06 – SOC Dashboards](06-SOC-Dashboards/)                                               | Wazuh and Security Onion monitoring dashboards                                                  |
| [07 – Supporting Reference](07-Supporting-Reference/)                                   | Integration summaries, framework mapping, and supporting documentation                          |
| [08 – Penetration Testing](08-Penetration-Testing/)                                     | Rekall penetration test portfolio report                                                        |
| [09 – Cloud Security](09-Cloud-Security/01-Azure-IAM/)                                  | Azure IAM investigations using Azure RBAC, Activity Logs, and Microsoft Entra logs              |
| [Integration References](07-Supporting-Reference/Integrations/)                         | Wazuh, Shuffle, TheHive, Zammad, and Slack integration summaries                                |
| [Framework and Control Mapping](07-Supporting-Reference/Framework-and-Control-Mapping/) | MITRE ATT&CK, NIST CSF, OWASP, ISO concepts, and control gaps                                   |

---

## Investigation Coverage

### Windows and Active Directory

* PowerShell encoded-command execution
* Registry Run Key persistence
* WinRM lateral movement
* NTLM authentication activity

### Linux

* Sudo privilege activity
* SSH authentication activity

### Network and Web Applications

* SQL injection
* Malicious file upload
* Network service discovery

The original nine cases are retained as **Pre-SOAR Investigations** because they were completed before implementation of the full Shuffle, TheHive, Zammad, and Slack lifecycle.

### Full-Lifecycle Investigation

* [OneDrive.exe Access to Explorer.exe](04-Investigations/02-Full-Lifecycle-Investigations/01-OneDrive-Explorer-Process-Access/) — Wazuh alert intake, Tier 1 triage, Tier 2 investigation, Detection Engineering handoff, Zammad ticket closure, TheHive handback, and final case closure

Additional cases may be repeated or extended through the same operational workflow as the lab develops.

---

## Cloud Security and IAM Investigations

The cloud-security workstream extends the portfolio beyond the local AESOC environment. These investigations use Microsoft Azure and Microsoft Entra evidence to analyze identity, permissions, authentication activity, containment actions, and recovery validation.

### Unauthorized Azure Contributor Assignment

[Unauthorized Azure Contributor Assignment](09-Cloud-Security/01-Azure-IAM/01-Unauthorized-Contributor-Assignment/)

* Established approved group-based Reader access as the baseline
* Verified that the analyst account could not modify resource-group tags
* Investigated a simulated direct Contributor assignment that bypassed the approved access model
* Used Azure Activity Logs to identify the role change, initiating administrator, affected identity, scope, and subsequent resource modification
* Removed excessive access, preserved approved Reader access, and confirmed that write access was denied after remediation

**Outcome:** Classified as a high-severity true positive within an authorized lab simulation. Least-privilege access was restored and the unauthorized configuration change was remediated.

### Suspicious Entra Sign-In and Account Containment

[Suspicious Entra Sign-In and Account Containment](09-Cloud-Security/01-Azure-IAM/02-Suspicious-Sign-In-Account-Containment/)

* Investigated three failed password attempts followed by a successful MFA-authenticated Azure Portal sign-in
* Reviewed Microsoft Entra sign-in details, authentication requirements, application context, timestamps, and failure codes
* Disabled the affected account, revoked active sessions, and reset the password
* Confirmed that sign-in was blocked while the account was contained
* Re-enabled the account after remediation and validated successful MFA-protected recovery

**Outcome:** Classified as a medium-severity true positive within an authorized lab simulation. Account access was contained, credentials and sessions were remediated, and secure access was restored.

---

## Detection Engineering

AESOC currently includes two completed detection projects and one full-lifecycle detection review:

### Custom Detection

[Windows Discovery Activity Detection](05-Detection-Engineering/Custom-Detections/Custom-Detection-001-Windows-Discovery-Activity/)

* Identified a Wazuh detection gap
* Confirmed that the required telemetry was available
* Developed a custom Wazuh rule
* Generated controlled test activity
* Validated successful alert generation

### Detection Tuning

[PowerShell Script Policy Test Reclassification](05-Detection-Engineering/Detection-Tuning/Detection-Tuning-001-Windows-PowerShell-Activity/)

* Investigated repeated high-severity alerts
* Confirmed legitimate PowerShell behavior
* Created a child rule to reduce severity
* Preserved event visibility
* Retested and validated the tuning result

### Full-Lifecycle Detection Review

[OneDrive.exe Access to Explorer.exe](04-Investigations/02-Full-Lifecycle-Investigations/01-OneDrive-Explorer-Process-Access/03-Detection-Engineering/)

* Confirmed repeated Level 12 alert noise from expected application behavior
* Reviewed Wazuh Rule `92910` and proposed a narrowly scoped child rule
* Passed controlled positive and negative condition tests
* Documented unsuccessful live validation
* Deferred the production change without reducing existing detection coverage

---

## Security Stack

### Infrastructure and Segmentation

* Raspberry Pi portable Internet gateway
* OPNsense firewall and inter-VLAN routing
* Netgear GS108Tv3 managed switch
* Proxmox VE virtualization
* Dedicated Management, Active Directory, Quarantine, DMZ, and Security VLANs

### Detection and Monitoring

* Wazuh
* Security Onion
* Sysmon
* Windows Event Logs
* Auditd
* Syslog
* Suricata
* Zeek

### Cloud Security and Identity

* Microsoft Azure
* Microsoft Entra ID
* Azure RBAC
* Azure Activity Logs
* Microsoft Entra sign-in logs
* Microsoft Entra audit logs
* Microsoft Authenticator and MFA

### Security Operations

* Shuffle SOAR
* TheHive
* Zammad
* Slack
* Wazuh dashboards
* Security Onion dashboards

### Monitored and Test Systems

* Windows Server 2025 domain controller
* Windows 10 endpoint
* Rocky Linux endpoint
* Microsoft Entra test identities
* Azure cloud-security resource group
* Quarantine network

---

## Skills Demonstrated

### IT Operations Skills

Building and operating AESOC required hands-on IT administration and troubleshooting across:

* Windows Server 2025 and Active Directory
* Windows 10/11 and Rocky Linux endpoints
* Domain membership, user accounts, authentication, and access services
* Proxmox VE virtualization and virtual machine administration
* OPNsense routing, firewall rules, VLANs, and network segmentation
* Endpoint-agent deployment, configuration, and health validation
* Connectivity, authentication, service, telemetry, and API integration troubleshooting
* Zammad ticket routing, ownership assignment, status tracking, escalation, and closure documentation
* Technical runbooks, architecture diagrams, troubleshooting records, and GitHub documentation

### Security Operations Skills

* Alert triage and prioritization
* Endpoint and network investigation
* SIEM and network security monitoring analysis
* Log, event, and telemetry correlation
* MITRE ATT&CK mapping
* Detection engineering and validation
* Detection tuning and false-positive reduction
* SOAR workflow development
* Case lifecycle management
* Analyst ownership, escalation, and handoffs
* Dashboard development
* Controlled adversary simulation
* Azure RBAC and least-privilege analysis
* Cloud role-assignment investigation
* Microsoft Entra sign-in and audit-log analysis
* Identity containment, session revocation, credential reset, and MFA recovery validation

---

## Framework and Control Mapping

AESOC documentation connects completed lab work to:

* MITRE ATT&CK
* NIST Cybersecurity Framework 2.0
* OWASP concepts
* ISO/IEC 27001 concepts
* Detection and monitoring control-gap analysis

These mappings describe how the implemented technical work relates to broader security operations and risk-management concepts. They do not claim formal organizational compliance or certification.

[Review the framework mappings](07-Supporting-Reference/Framework-and-Control-Mapping/)

---

## Project Background

AESOC began after completing The Ohio State University Cybersecurity Bootcamp. I wanted to understand not only how to perform individual security exercises, but also how the surrounding infrastructure collects telemetry, generates detections, supports investigations, and moves security work through an operational lifecycle.

The result is a personally designed and operated security portfolio that combines the AESOC home lab with Windows, Linux, web-application, and network telemetry, plus separate Azure IAM investigations using cloud identity, access, sign-in, and audit evidence.
