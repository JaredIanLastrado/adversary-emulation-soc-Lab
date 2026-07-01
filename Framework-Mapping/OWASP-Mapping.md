# OWASP Mapping

This document maps AESOC web application investigations to OWASP web security concepts.

OWASP concepts are used in AESOC to support analysis of web application attack activity, especially SQL injection and malicious file upload behavior.

## OWASP-Aligned Investigations

| AESOC Case | OWASP Concept | What Was Investigated |
|---|---|---|
| Case 007 - SQL Injection Investigation | SQL Injection | Reviewed HTTP request evidence containing a SQL injection payload and validated Security Onion alert evidence |
| Case 008 - Malicious File Upload Investigation | Malicious File Upload / Unrestricted File Upload Risk | Reviewed HTTP POST activity, uploaded PHP content, and Security Onion alert evidence |

## Case 007 - SQL Injection

The SQL Injection investigation reviewed web attack activity against DVWA. The investigation focused on:

- Source and destination IP addresses
- HTTP request path
- SQL injection payload evidence
- Security Onion alert details
- Attack classification
- MITRE ATT&CK mapping
- Remediation recommendations

## Case 008 - Malicious File Upload

The malicious file upload investigation reviewed suspicious upload activity against DVWA. The investigation focused on:

- HTTP POST activity
- Uploaded PHP file evidence
- Security Onion alert details
- Source and destination context
- Attack classification
- Remediation recommendations

## Summary

AESOC applies OWASP concepts by analyzing web application attack evidence in a controlled lab environment. The project does not claim to implement a full OWASP testing program, but it demonstrates hands-on investigation of common web application attack patterns.
