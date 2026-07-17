# Wazuh and Shuffle Integration

## Purpose

This integration forwards selected Wazuh alerts to Shuffle for automated severity routing, TheHive alert creation, and Slack notification.

## Data Flow

```text
Endpoint Activity
        ↓
Wazuh Agent
        ↓
Wazuh Alert
        ↓
Shuffle Webhook
        ↓
Severity Branch
        ↓
TheHive Alert + Slack Notification
```

## Severity Routing

| Wazuh rule level | AESOC classification |
|---:|---|
| 7–11 | Medium |
| 12–14 | High |
| 15 or higher | Critical |

## Alert Data Used

Depending on the event, Shuffle may receive:

- Rule ID
- Rule description
- Rule level
- Timestamp
- Alert identifier
- Agent name
- Agent IP address
- Process or command-line information
- User information
- MITRE ATT&CK mapping

Not every Wazuh alert contains every field.

## Result

Shuffle creates a structured TheHive alert and posts a formatted message to `#aesoc-alerts`.

The alert is then available for Tier 1 triage.

## Validation

The integration was validated using a Wazuh level `12` alert.

The test confirmed that:

- Wazuh delivered the alert to Shuffle.
- Shuffle selected the high-severity branch.
- TheHive created the alert.
- Slack received the notification.

See:

[01 – Wazuh Alert Intake](../../03-SOAR-Automation/01-Wazuh-Alert-Intake/)

## Security Notes

- Webhook URLs and API credentials are not published.
- Screenshots and payloads use controlled lab data.
- Severity routing is an initial automation decision and not a final analyst classification.
