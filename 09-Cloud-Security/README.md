# Cloud Security

This section documents the Microsoft Azure cloud-security portion of the AESOC portfolio.

The projects cover identity and access management investigations, Microsoft Entra authentication analysis, Azure RBAC, least-privilege validation, Microsoft Sentinel detection development, cloud alert triage, containment, and incident closure.

## Project Areas

| Area | Focus |
| --- | --- |
| [Azure IAM](01-Azure-IAM/) | Evidence-backed investigations involving Azure RBAC, Microsoft Entra sign-ins, account containment, access remediation, and recovery validation |
| [Microsoft Sentinel](02-Microsoft-Sentinel/) | Azure Activity ingestion, KQL detection development, scheduled analytics rules, alert generation, incident investigation, and resolution |

## Featured Projects

### Azure IAM Investigations

| Project | What it demonstrates |
| --- | --- |
| [Unauthorized Azure Contributor Assignment](01-Azure-IAM/01-Unauthorized-Contributor-Assignment/) | Approved-access baselining, excessive-permission investigation, Activity Log attribution, containment, and least-privilege restoration |
| [Suspicious Entra Sign-In and Account Containment](01-Azure-IAM/02-Suspicious-Sign-In-Account-Containment/) | Sign-in analysis, MFA validation, account disablement, session revocation, password reset, and secure recovery |

### Microsoft Sentinel Detection

| Project | What it demonstrates |
| --- | --- |
| [Azure RBAC Role Assignment Detection](02-Microsoft-Sentinel/01-Azure-RBAC-Role-Assignment-Detection/) | KQL analytics, Azure Activity monitoring, account and IP entity mapping, alert generation, incident investigation, containment, and closure |

## Cloud Security Workflow

## Technologies

- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Azure Activity Logs
- Microsoft Entra sign-in and audit logs
- Microsoft Sentinel
- Log Analytics
- Kusto Query Language
- Microsoft Defender portal
- Microsoft Authenticator and MFA

## Skills Demonstrated

- Azure RBAC and least-privilege analysis
- Cloud role-assignment investigation
- Microsoft Entra authentication analysis
- Account and session containment
- Access remediation and recovery validation
- KQL query development
- Microsoft Sentinel analytics-rule configuration
- Cloud alert and incident investigation
- MITRE ATT&CK mapping
- Evidence-based incident documentation

> **Scope:** All identities, role assignments, sign-ins, alerts, incidents, and response actions were created in an authorized personal lab environment. These projects do not represent production incidents.
