# MITRE ATT&CK Mapping

This document summarizes how AESOC investigations map observed activity to MITRE ATT&CK tactics and techniques.

MITRE ATT&CK is used in AESOC to describe adversary behavior, organize findings, and communicate investigation results using a common security framework.

## Investigation Mapping

| Case | Activity | MITRE ATT&CK Technique | What Was Observed |
|---|---|---|---|
| Case 001 - PowerShell Encoded Command Execution | Suspicious PowerShell execution | T1059.001 - PowerShell | PowerShell command execution with encoded command behavior |
| Case 002 - Registry Run Key Persistence | Registry persistence | T1547.001 - Registry Run Keys / Startup Folder | Registry Run key modification used to simulate persistence |
| Case 003 - WinRM Lateral Movement | Remote access using WinRM | T1021.006 - Windows Remote Management | WinRM traffic and endpoint evidence associated with PowerShell Remoting |
| Case 004 - NTLM Authentication Investigation | Authentication failure activity | T1110 - Brute Force / T1078 - Valid Accounts | Failed NTLM authentication evidence from endpoint and network sources |
| Case 005 - Sudo Privilege Investigation | Linux privilege escalation | T1548.003 - Sudo and Sudo Caching | Sudo activity showing elevated command execution |
| Case 006 - SSH Authentication Investigation | SSH authentication activity | T1110 - Brute Force | Failed SSH authentication attempts against a Linux endpoint |
| Case 007 - SQL Injection Investigation | Web application attack | T1190 - Exploit Public-Facing Application | SQL injection payload observed in HTTP request evidence |
| Case 008 - Malicious File Upload Investigation | Malicious web upload activity | T1190 - Exploit Public-Facing Application | PHP file upload activity observed through Security Onion evidence |
| Case 009 - Network Service Discovery Investigation | Network scanning and service discovery | T1046 - Network Service Discovery | Nmap activity and scan alerts against a honeypot target |

## Detection Engineering Mapping

| Project | Technique | What Was Built |
|---|---|---|
| Windows Discovery Activity Detection | T1033, T1016, T1082 | Custom Wazuh detection for whoami, ipconfig, and systeminfo |
| PowerShell Detection Tuning | T1059.001-related telemetry | Tuned benign PowerShell Script Policy Test activity to reduce false-positive severity |

## Summary

MITRE ATT&CK is used throughout AESOC to connect technical evidence to adversary behavior. Each case includes technique mapping to support clearer incident documentation and analyst communication.
