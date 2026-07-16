# Wazuh Alert Intake – Validation Evidence

This directory contains evidence demonstrating successful execution of the AESOC Wazuh Alert Intake playbook.

The documented test used a Wazuh level 12 alert, which was expected to follow the high-severity branch.

## Evidence Files

| File | What it demonstrates |
|---|---|
| [`01-High-Branch-Conditions.png`](01-High-Branch-Conditions.png) | Shuffle evaluates whether the Wazuh rule level is greater than 11 and less than 15 |
| [`02-TheHive-Alert-Details.png`](02-TheHive-Alert-Details.png) | The alert was created in TheHive and received the Wazuh alert metadata |
| [`03-Slack-High-Alert-Notification.png`](03-Slack-High-Alert-Notification.png) | The AESOC SOAR Agent delivered the high-severity notification to Slack |

## Validated Execution Path

```text
Wazuh Level 12 Alert
        ↓
Shuffle Webhook
        ↓
High-Severity Conditions Matched
        ↓
TheHive Alert Created
        ↓
Slack Notification Delivered
