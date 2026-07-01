# Security Operations

This directory contains planned AESOC security operations projects that expand the lab beyond alert investigation, detection engineering, and dashboard development.

---

## Project Status

| Status      | Meaning                                                |
| ----------- | ------------------------------------------------------ |
| Planned     | Project has been scoped but not started                |
| In Progress | Evidence collection or documentation is underway       |
---

## Planned Security Operations Projects

| Project                            | Status  | Focus Area                                                                             | Planned Deliverables                                                                                                          |
| ---------------------------------- | ------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Identity Compromise Triage         | In Progress | Active Directory authentication investigation and account compromise analysis          | Timeline, screenshots, Wazuh evidence, AD event evidence, MITRE ATT&CK mapping, final incident report                         |
| Ticketing and Escalation Workflow  | Planned | SOC ticket documentation, severity classification, escalation, and SLA handling        | Ticket templates, completed ticket examples, escalation matrix, SLA/severity guide                                            |
| SOC Alert Summary Automation       | Planned | Python-based alert summary parsing and SOC metrics reporting                           | Python script, sample input, sample output, README documentation                                                              |
| IAM Access Lifecycle               | Planned | User provisioning, group membership review, deprovisioning, and access review evidence | User creation evidence, group membership evidence, account disable evidence, access review spreadsheet, access review writeup |
| Vulnerability Management Lifecycle | Planned | Vulnerability tracking, prioritization, remediation, validation, and reporting         | Scan result, risk register, remediation tracker, CVE/CVSS analysis, validation note, executive summary                        |

---

## Planned CVE / CVSS Inclusion

CVE and CVSS analysis will be included in the planned Vulnerability Management Lifecycle project.

| Concept     | Purpose                                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| CVE         | Identifies a publicly disclosed cybersecurity vulnerability                                                 |
| CVSS        | Provides a severity score for a vulnerability                                                               |
| Risk Rating | Prioritizes remediation based on severity, asset criticality, exposure, exploitability, and business impact |

Planned CVE/CVSS documentation will include:

* CVE ID
* CVSS version
* CVSS base score
* Severity rating
* Affected asset
* Evidence source
* Remediation recommendation
* Validation method
* Validation result

