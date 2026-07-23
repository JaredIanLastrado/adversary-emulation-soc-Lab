# Full-Lifecycle Investigations

This directory contains AESOC cases documented through the complete alert-to-resolution workflow, including analyst ownership, escalation, external work tracking, validation, and closure.

## Completed Investigations

| Investigation | Detection source | Escalation path | Final disposition |
|---|---|---|---|
| [OneDrive.exe Access to Explorer.exe](01-OneDrive-Explorer-Process-Access/) | Wazuh Rule `92910` / Sysmon Event ID 10 | Tier 1 → Tier 2 → Detection Engineering | True Positive — Benign Application Behavior; tuning deferred |

## Documented Lifecycle

```text
Wazuh Alert
    ↓
Shuffle Intake
    ↓
TheHive Alert
    ↓
Tier 1 Triage
    ↓
Tier 2 Investigation
    ↓
Detection or Remediation Handoff
    ↓
Zammad Work Item
    ↓
TheHive Closure Handback
    ↓
Tier 2 Validation and Final Case Closure
```

Each investigation includes a complete overview, role-specific deep dives, repeatable queries, rule-testing artifacts, and supporting evidence.
