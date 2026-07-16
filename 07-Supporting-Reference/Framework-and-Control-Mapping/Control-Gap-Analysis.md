# Control Gap Analysis

This document summarizes control and visibility gaps identified during the AESOC project.

The purpose of this analysis is to show how hands-on SOC investigations, detection engineering, and dashboard development were used to identify gaps and improve monitoring coverage.

## Gap Analysis Summary

| Control Area | Gap Identified | Evidence | Improvement Made |
|---|---|---|---|
| Windows Discovery Detection | Commands such as whoami, ipconfig, and systeminfo were visible in Sysmon telemetry but did not generate a dedicated discovery alert | Detection Engineering 001 - Windows Discovery Activity | Built and validated a custom Wazuh detection rule |
| PowerShell Alert Quality | PowerShell Script Policy Test files generated critical-severity alerts despite being benign behavior | Detection Tuning 001 - Windows PowerShell Activity | Tuned alert severity from critical to low while preserving visibility |
| Endpoint and Network Correlation | Some cases required more than one telemetry source to confirm activity | WinRM Lateral Movement Investigation | Correlated Wazuh endpoint evidence with Security Onion network evidence |
| Web Attack Visibility | Web application attacks required HTTP request evidence, alert context, and source/destination review | SQL Injection and Malicious File Upload Investigations | Used Security Onion evidence to validate and document web attack activity |
| Monitoring Visibility | Raw alerts alone were not enough for quick triage and trend review | Wazuh and Security Onion dashboard projects | Built dashboards for endpoint and network monitoring visibility |

## NIST CSF 2.0 Category Relationship

| Gap Area | NIST CSF 2.0 Function | Why It Applies |
|---|---|---|
| Discovery detection gap | Detect | Improved detection coverage for suspicious discovery behavior |
| PowerShell false-positive noise | Detect / Govern | Improved alert quality and reduced analyst noise |
| Endpoint/network correlation | Detect / Respond | Improved investigation confidence through multiple evidence sources |
| Web attack visibility | Detect / Respond | Validated web attack evidence and documented findings |
| Dashboard visibility | Identify / Detect / Govern | Improved asset, alert, and monitoring visibility |

## Summary

AESOC demonstrates control gap identification through practical lab work. Gaps were identified by reviewing alerts, telemetry, dashboards, and investigation evidence. Improvements included custom detection engineering, detection tuning, endpoint/network correlation, and dashboard development.
