# TheHive and Shuffle Integration

## Purpose

This integration allows Shuffle to create and update TheHive records while also receiving TheHive case lifecycle events.

## Data Flow

```text
Shuffle → TheHive
Alert creation, comments, tags, and case updates
```

```text
TheHive → Shuffle
Case-created, case-updated, and case-closed events
```

## Shuffle Actions

Shuffle can:

- Create TheHive alerts
- Add case comments
- Add workflow tags
- Record Zammad ticket references
- Synchronize ticket-completion status

## TheHive Events

Shuffle processes events such as:

| Event | Automated result |
|---|---|
| Case created | Slack case-created notification |
| Case updated | Evaluate action-request tags |
| Case closed | Slack case-closed notification |

## Workflow Tags

Examples used by the AESOC workflows include:

- `detection-review-request`
- `detection-ticket-created`
- `detection-review-complete`
- `infrastructure-remediation-request`

Tags allow Shuffle to determine which workflow path should execute and help prevent duplicate ticket creation.

## System of Record

TheHive remains the primary record for:

- Alert details
- Investigation findings
- Case ownership
- Comments
- Evidence
- Workflow status
- Closure rationale

## Validation

The integration was validated through:

- Wazuh alert creation
- Case-event notifications
- Zammad ticket routing
- Ticket-completion handback

See:

- [01 – Wazuh Alert Intake](../../03-SOAR-Automation/01-Wazuh-Alert-Intake/)
- [02 – Case Updates and Ticket Routing](../../03-SOAR-Automation/02-Case-Updates-and-Ticket-Routing/)
- [03 – Ticket Closure Handback](../../03-SOAR-Automation/03-Ticket-Closure-Handback/)

## Security Notes

- TheHive API credentials and webhook URLs are not published.
- Routing depends on consistent case tags and metadata.
- Automation updates do not replace analyst validation.
