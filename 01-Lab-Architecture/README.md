# 01 – AESOC Lab Architecture

## Overview

The AESOC Lab is a segmented cybersecurity home lab designed to simulate an entry-level Security Operations Center environment.

The architecture combines:

- Endpoint telemetry collection
- Network traffic monitoring
- Host-based and network-based detection
- SOAR automation
- Alert and case management
- Detection and remediation ticketing
- Controlled attack simulations
- Quarantine and network segmentation

The lab is hosted primarily through Proxmox VE and separated into dedicated VLANs using OPNsense and a managed switch.

---

# AESOC Network Architecture

The diagram below shows the physical and logical layout of the AESOC Lab.

[![AESOC Network Architecture](AESOC-Architecture.png)](AESOC-Architecture.png)

[Open the architecture diagram at full size](AESOC-Architecture.png)

## Core Infrastructure

```text
Internet
   ↓
Raspberry Pi Internet Gateway
   ↓
OPNsense Firewall and Router
   ↓
Netgear GS108Tv3 Managed Switch
   ↓
AESOC VLANs
```

The Raspberry Pi acts as the portable lab's Internet gateway.

It connects to the available wireless network and provides Internet connectivity to OPNsense. This allows the AESOC server environment to remain portable without requiring a direct Ethernet connection to the home router.

| Component | Address | Role |
|---|---|---|
| Raspberry Pi Internet Gateway | `10.0.0.1` | Connects the portable lab to external Wi-Fi |
| OPNsense | `172.16.110.1` | Firewall, gateway, and inter-VLAN router |
| Netgear GS108Tv3 | `172.16.110.20` | Managed switch and VLAN transport |
| Proxmox VE | `172.16.110.60` | Hosts the AESOC virtual machines |

---

# Network and VLAN Overview

The AESOC Lab uses VLAN segmentation to separate administrative systems, monitored endpoints, security platforms, vulnerable services, and quarantined hosts.

OPNsense controls routing between the VLANs and enforces network separation.

## VLAN Summary

| VLAN | Subnet | Purpose | Main assets |
|---:|---|---|---|
| 110 | `172.16.110.0/24` | Management Network | Windows 11 administrator, Proxmox VE, OPNsense, managed switch |
| 120 | `172.16.120.0/24` | Active Directory Network | Windows Server 2025, Windows 10, Rocky Linux |
| 130 | `172.16.130.0/24` | Quarantine Network | Suspected or compromised hosts |
| 140 | `172.16.140.0/24` | Demilitarized Zone | Security Onion IDH, DVWA |
| 150 | `172.16.150.0/24` | Security Network | Security Onion, Wazuh, Shuffle, TheHive, Zammad |

---

## VLAN 110 – Management Network

**Subnet:** `172.16.110.0/24`

The Management Network contains systems used to administer the AESOC infrastructure.

| Asset | Address | Purpose |
|---|---|---|
| OPNsense | `172.16.110.1` | Firewall, router, and VLAN gateway |
| Windows 11 Administrator | `172.16.110.2` | Administrative workstation |
| Managed Switch | `172.16.110.20` | VLAN transport and physical connectivity |
| Proxmox VE | `172.16.110.60` | Virtualization host |

Administrative access is separated from endpoint, vulnerable-application, and security-monitoring traffic.

---

## VLAN 120 – Active Directory Network

**Subnet:** `172.16.120.0/24`

This VLAN represents a small enterprise-style endpoint environment.

| Asset | Address | Purpose |
|---|---|---|
| Windows Server 2025 | `172.16.120.30` | Active Directory domain controller |
| Windows 10 | DHCP | Domain-joined Windows endpoint |
| Rocky Linux | DHCP | Domain-connected Linux endpoint |

The Windows and Linux systems generate endpoint telemetry that is forwarded to Wazuh.

Their network traffic may also be mirrored to Security Onion for passive monitoring.

---

## VLAN 130 – Quarantine Network

**Subnet:** `172.16.130.0/24`

The Quarantine Network is reserved for suspected or compromised systems that require isolation from the normal lab environment.

Quarantined systems may retain limited connectivity for:

- Security monitoring
- Evidence collection
- Analyst validation
- Approved remediation activity
- Post-containment observation

They should not receive unrestricted access to the Management, Active Directory, DMZ, or Security networks.

---

## VLAN 140 – Demilitarized Zone

**Subnet:** `172.16.140.0/24`

The DMZ contains intentionally exposed or vulnerable systems used for controlled security testing.

| Asset | Address | Purpose |
|---|---|---|
| Security Onion IDH | `172.16.140.2` | Intrusion Detection Honeypot deployment |
| DVWA | `172.16.140.3` | Intentionally vulnerable web application |

The Security Onion IDH and main Security Onion platform are separate deployments.

The IDH provides a monitored target for reconnaissance and unauthorized-connection activity, while the main Security Onion system in VLAN 150 performs network security monitoring.

---

## VLAN 150 – Security Network

**Subnet:** `172.16.150.0/24`

The Security Network contains the platforms responsible for detection, automation, case management, and ticketing.

| Asset | Address | Purpose |
|---|---|---|
| Security Onion | `172.16.150.2` | Main network security monitoring platform |
| Wazuh | `172.16.150.20` | SIEM and endpoint detection platform |
| Zammad | `172.16.150.30` | Detection and remediation ticketing |
| TheHive | `172.16.150.50` | Alert and case management |
| Shuffle | `172.16.150.60` | SOAR automation and orchestration |

---

## Segmentation Principles

The network design follows these principles:

- Administrative systems are separated from attack-simulation targets.
- Vulnerable DMZ services are isolated from management systems.
- Security platforms are grouped inside a dedicated security VLAN.
- Suspected or compromised systems can be moved into a restricted quarantine network.
- Inter-VLAN traffic is controlled by OPNsense.
- Only required monitoring, administration, and integration traffic is permitted.
- The portable Internet gateway remains separated from the internal AESOC VLAN structure.

For a standalone version of this section, see:

[Network and VLAN Overview](Network-and-VLAN-Overview.md)

---

# Telemetry Flow

The following diagram shows how endpoint logs, network traffic, alerts, case metadata, ticket metadata, and notifications move through the AESOC Lab.

[![AESOC Telemetry Flow](Telemetry-Flow.png)](Telemetry-Flow.png)

[Open the telemetry-flow diagram at full size](Telemetry-Flow.png)

## Host Telemetry Pipeline

```text
Windows Server 2025
Windows 10
Rocky Linux
        ↓
Endpoint Logs and Events
        ↓
Wazuh Agents
        ↓
Wazuh Server
        ↓
Wazuh Rule Evaluation
        ↓
Wazuh Alerts
```

Wazuh provides the primary host-based visibility for the AESOC Lab.

It receives:

- Sysmon telemetry
- Windows Event Logs
- Wazuh Agent data
- Linux auditd events
- Linux syslog events

Wazuh evaluates the incoming events against built-in and custom rules and generates alerts when configured conditions are matched.

---

## Network Telemetry Pipeline

```text
Endpoint-Generated Network Traffic
DMZ Traffic
DVWA Web Traffic
Security Onion IDH Traffic
        ↓
Selected Traffic Mirroring
        ↓
Proxmox TC Rules
        ↓
Security Onion
        ↓
Zeek and Suricata Analysis
        ↓
Security Onion Alerts
```

Selected network traffic is mirrored through traffic-control rules in Proxmox and forwarded to the Security Onion monitoring interface.

Security Onion provides:

- Network visibility
- Suricata alerts
- Zeek connection and protocol metadata
- Packet and session analysis
- Web-attack visibility
- Reconnaissance detection
- Suspicious network-activity investigation

Security Onion alerts remain within the Security Onion platform because a direct Security Onion-to-Shuffle integration has not yet been implemented.

---

## Alert, Case, and Ticket Metadata Flow

Selected Wazuh alert metadata continues into the SOAR automation pipeline.

```text
Wazuh Alert Metadata
        ↓
Shuffle
        ↓
TheHive Alert Creation
        ↓
TheHive Case Lifecycle Events
        ↓
Shuffle
        ↓
Zammad Ticket Creation
        ↓
Zammad Ticket Closure Event
        ↓
Shuffle
        ↓
TheHive Case Update
```

Shuffle also sends formatted notifications to Slack:

```text
Shuffle
   ↓
Slack Alert, Case, and Ticket Notifications
```

The documented SOAR flow is implemented through:

- `01-Wazuh-Alert-Intake`
- `02-Case-Updates-and-Ticket-Routing`
- `03-Ticket-Closure-Handback`

TheHive remains the primary alert and case record.

Zammad tracks Detection Engineering and Infrastructure Remediation work.

Slack provides operational visibility but is not used as the official investigation record.

---

# Asset and Telemetry Matrix

The following matrix identifies the major AESOC assets, the telemetry they generate or process, and the platforms responsible for monitoring them.

## Endpoint and Network Assets

| Asset | Network | Role | Telemetry | Primary platform | Main visibility |
|---|---|---|---|---|---|
| Windows Server 2025 | VLAN 120 | Active Directory domain controller | Sysmon, Windows Event Logs, Wazuh Agent | Wazuh | Account changes, authentication, process execution, persistence, privilege activity |
| Windows 10 | VLAN 120 | Domain-joined Windows endpoint | Sysmon, Windows Event Logs, Wazuh Agent | Wazuh | Process creation, PowerShell, registry activity, authentication, lateral movement |
| Rocky Linux | VLAN 120 | Linux domain client | Auditd, syslog, Wazuh Agent | Wazuh | Authentication, sudo activity, command execution, service activity |
| Quarantined Host | VLAN 130 | Suspected or compromised endpoint | Endpoint telemetry and network traffic when available | Wazuh and Security Onion | Continued activity after containment |
| DVWA | VLAN 140 | Vulnerable web application | HTTP requests and network traffic | Security Onion | Web attacks, scanning, brute-force attempts, application exploitation |
| Security Onion IDH | VLAN 140 | Intrusion Detection Honeypot | Connection and network activity | Security Onion | Reconnaissance, unauthorized connections, suspicious traffic |

---

## Security and Operations Platforms

| Platform | Address | Function | Receives | Produces |
|---|---|---|---|---|
| Wazuh | `172.16.150.20` | SIEM and endpoint detection | Sysmon, Windows logs, auditd, syslog, Wazuh Agent telemetry | Endpoint alerts and rule matches |
| Security Onion | `172.16.150.2` | Network security monitoring | Mirrored network traffic | Suricata alerts, Zeek metadata, packet and session evidence |
| Shuffle | `172.16.150.60` | SOAR automation | Wazuh, TheHive, and Zammad webhook events | Routed alerts, ticket actions, comments, tags, and Slack notifications |
| TheHive | `172.16.150.50` | Alert and case management | Structured alerts and analyst findings | Alerts, cases, tasks, comments, tags, and closure records |
| Zammad | `172.16.150.30` | Action and remediation ticketing | Ticket requests created through Shuffle | Detection-review and infrastructure-remediation work records |
| Slack | Cloud service | Operational notification | Formatted messages from Shuffle | Alert, case, and ticket visibility |

---

## Telemetry Coverage by Activity

| Activity | Primary telemetry | Detection or management platform |
|---|---|---|
| Windows process execution | Sysmon Event ID 1 | Wazuh |
| PowerShell activity | Windows and Sysmon events | Wazuh |
| Registry persistence | Sysmon registry events | Wazuh |
| Windows authentication | Windows Security Events | Wazuh |
| Active Directory account changes | Domain controller security events | Wazuh |
| Linux authentication | Syslog and auditd | Wazuh |
| Linux privilege use | Auditd and sudo logs | Wazuh |
| Web-application attacks | HTTP and network traffic | Security Onion |
| Network scanning | Zeek and Suricata data | Security Onion |
| Suspicious network sessions | Zeek, Suricata, and packet evidence | Security Onion |
| Alert routing | Wazuh alert metadata | Shuffle |
| Alert and case records | Cases, comments, tags, and tasks | TheHive |
| Detection or remediation actions | Ticket metadata and status | Zammad |
| Operational notifications | Alert, case, and ticket summaries | Slack |

---

## Coverage Notes

- Wazuh provides the primary host-based monitoring capability.
- Security Onion provides the primary network-based monitoring capability.
- Shuffle automates selected Wazuh, TheHive, Zammad, and Slack workflows.
- Security Onion alerts are reviewed within Security Onion unless a future automation integration is added.
- TheHive is the primary alert and investigation system of record.
- Zammad records work assigned to Detection Engineering or IT/Remediation.
- Slack is used for visibility and notification rather than formal recordkeeping.

For a standalone version of this section, see:

[Asset and Telemetry Matrix](Asset-and-Telemetry-Matrix.md)

---

# Attack Simulation Flow

The following diagram shows how controlled attack activity is generated against endpoint and network targets and how those activities produce Wazuh and Security Onion alerts.

[![AESOC Attack Simulation Flow](Attack-Simulation-Flow.png)](Attack-Simulation-Flow.png)

[Open the attack-simulation diagram at full size](Attack-Simulation-Flow.png)

## Controlled Simulation Paths

### Endpoint attacks

```text
Controlled Manual Attack
        ↓
Windows Server 2025
Windows 10
Rocky Linux
        ↓
Host-Based Telemetry
        ↓
Wazuh
        ↓
Host Alerts
```

Example endpoint activity includes:

- PowerShell execution
- Registry persistence
- Windows account activity
- WinRM lateral movement
- Linux authentication activity
- Privilege escalation
- Command execution
- Discovery activity

### Network attacks

```text
Controlled Network Activity
        ↓
DVWA
Security Onion IDH
Selected Network Traffic
        ↓
Mirrored Network Traffic
        ↓
Security Onion
        ↓
Network Alerts
```

Example network activity includes:

- Web-application attacks
- SQL injection
- Brute-force attempts
- Reconnaissance
- Network scanning
- Unauthorized connection attempts
- Suspicious traffic patterns

The resulting host and network alerts are available for investigation and alert-lifecycle processing.

---

# AESOC Security Stack

## Infrastructure and Network Control

| Technology | Role in AESOC |
|---|---|
| Raspberry Pi | Portable wireless Internet gateway |
| OPNsense | Firewall, inter-VLAN routing, segmentation, and traffic-control enforcement |
| Proxmox VE | Virtualization platform hosting AESOC servers and endpoints |
| Netgear GS108Tv3 | Managed switching and VLAN transport |

---

## Telemetry Collection

| Technology | Role in AESOC |
|---|---|
| Sysmon | Detailed Windows process, network, file, registry, and process-access telemetry |
| Windows Event Logs | Authentication, account management, service, policy, and operating-system events |
| Wazuh Agent | Forwards Windows and Linux telemetry to Wazuh |
| Auditd | Captures Linux process, command, authentication, and security-relevant system activity |
| Syslog | Supplies Linux authentication, service, and system messages |
| Proxmox TC traffic mirroring | Forwards selected network traffic to Security Onion |

---

## Detection and Monitoring

### Wazuh

**Role:** Host-based SIEM and endpoint detection platform

Wazuh provides:

- Windows endpoint monitoring
- Linux endpoint monitoring
- Sysmon analysis
- Windows Event Log analysis
- Auditd and syslog analysis
- Rule-based alert generation
- Custom detection development
- Alert forwarding to Shuffle

### Security Onion

**Role:** Network security monitoring platform

Security Onion provides:

- Network traffic visibility
- Suricata alerting
- Zeek network metadata
- Packet and session analysis
- Web-attack monitoring
- Reconnaissance detection
- Network-anomaly investigation

---

## Security Operations and Automation

### Shuffle SOAR

**Role:** Workflow automation and integration

Shuffle automates:

- Wazuh alert intake
- Severity-based alert routing
- TheHive alert creation
- Slack alert notifications
- TheHive case lifecycle notifications
- Detection Engineering ticket creation
- Infrastructure Remediation ticket creation
- Zammad ticket-closure handback
- TheHive case comments and workflow tags

### TheHive

**Role:** Alert and case management

TheHive is used for:

- Alert review
- Case creation
- Investigation tasks
- Analyst findings
- Workflow tags
- Case comments
- Ticket references
- Case closure

TheHive is the primary investigation system of record.

### Zammad

**Role:** Detection and remediation ticketing

Zammad tracks:

- Detection Engineering review requests
- Infrastructure Remediation requests
- Assigned ownership
- Work-item status
- Ticket completion
- Ticket closure

### Slack

**Role:** Operational notification

Slack provides visibility into:

- Wazuh alerts
- TheHive case creation and closure
- Zammad ticket creation
- Ticket completion
- Case handback

Slack is not treated as the authoritative investigation record.

---

## Vulnerable and Monitored Systems

| System | Purpose |
|---|---|
| Windows Server 2025 | Active Directory and Windows security-event generation |
| Windows 10 | Windows endpoint simulation and investigation |
| Rocky Linux | Linux authentication, process, and privilege monitoring |
| DVWA | Web-application attack simulation |
| Security Onion IDH | Reconnaissance and unauthorized-connection monitoring |
| Quarantine VLAN | Isolation and post-containment monitoring |

---

## End-to-End Security Operations Flow

```text
Endpoint Activity
      ↓
Wazuh
      ↓
Shuffle
      ↓
TheHive Alert and Slack Notification
      ↓
Case Investigation
      ↓
Detection or Remediation Request
      ↓
Zammad Ticket
      ↓
Ticket Closure
      ↓
Shuffle Handback
      ↓
TheHive Update and Slack Notification
      ↓
Tier 2 Validation and Case Closure
```

Network monitoring follows a separate detection path:

```text
Network Activity
      ↓
Mirrored Traffic
      ↓
Security Onion
      ↓
Network Alerts and Investigation Data
```

---

## Platform Responsibility Summary

| Responsibility | Platform or owner |
|---|---|
| Host-based detection | Wazuh |
| Network-based detection | Security Onion |
| Workflow automation | Shuffle |
| Alert and case management | TheHive |
| Detection and remediation ticketing | Zammad |
| Operational notifications | Slack |
| Initial alert validation | Tier 1 SOC Analyst |
| Investigation and response | Tier 2 SOC Analyst |
| Detection-rule review | Detection Engineering |
| Infrastructure changes | IT/Remediation |

For a standalone version of this section, see:

[Security Stack](Security-Stack.md)

---

# Architecture Design Summary

The AESOC architecture separates the major security functions:

- **Endpoints and vulnerable systems** generate activity.
- **Wazuh and Security Onion** provide detection and visibility.
- **Shuffle** automates data movement and system integration.
- **TheHive** stores alerts, cases, findings, comments, and closure decisions.
- **Zammad** tracks specialized Detection Engineering and remediation work.
- **Slack** provides operational visibility.
- **OPNsense and VLANs** enforce network separation.
- **Proxmox** hosts the virtualized lab environment.
- **The Raspberry Pi gateway** provides portable wireless Internet connectivity.

This design supports both controlled adversary simulation and SOC-style alert-to-resolution workflows while keeping each platform's responsibility clearly defined.
