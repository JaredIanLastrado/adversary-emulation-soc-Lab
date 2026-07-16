# ISO/IEC 27001 Concepts Mapping

This document maps current AESOC activities to foundational ISO/IEC 27001 information security management concepts.

AESOC does not implement or certify an ISO/IEC 27001 Information Security Management System. This mapping is included to show how current lab work relates to common security control concepts such as access control, logging, monitoring, incident management, and evidence collection.

## Concept Mapping

| ISO/IEC 27001 Concept | AESOC Activity | Evidence in Repository |
|---|---|---|
| Access Control | Active Directory environment used for domain users, authentication activity, NTLM investigation, and WinRM lateral movement simulation | AD-related investigations, NTLM Authentication Investigation, WinRM Lateral Movement Investigation |
| Logging and Monitoring | Endpoint and network telemetry collected through Wazuh, Security Onion, Sysmon, Auditd, Suricata, and Zeek | Investigations, Dashboards, Telemetry Flow Diagram |
| Incident Management | Security events investigated using alert triage, detection validation, evidence review, findings, lessons learned, and classification | Investigation case folders |
| Evidence Collection | Screenshots and telemetry artifacts documented for each investigation | Case screenshots and README files |
| Vulnerability and Web Risk Awareness | SQL injection and malicious file upload activity investigated in DVWA | SQL Injection Investigation, Malicious File Upload Investigation |
| Continuous Improvement | Detection gaps and noisy alerts reviewed and improved through detection engineering and tuning | Windows Discovery Detection Engineering, PowerShell Detection Tuning |

## Summary

This mapping shows foundational exposure to ISO/IEC 27001-related security concepts. The strongest current AESOC alignments are logging and monitoring, incident documentation, access control evidence, and continuous improvement through detection tuning.
