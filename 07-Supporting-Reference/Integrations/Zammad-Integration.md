# Zammad Integration

## Purpose

Zammad tracks Detection Engineering and Infrastructure Remediation work that must be completed outside the immediate Tier 2 investigation.

## Ticket Categories

| Ticket type | Owner |
|---|---|
| Detection Review | Detection Engineering |
| Infrastructure Remediation | Infrastructure Operations or IT/Remediation |

## Ticket-Creation Flow

```text
Tier 2 Requests Action
        ↓
TheHive Workflow Tag
        ↓
Shuffle
        ↓
Zammad Ticket Created
        ↓
Ticket Reference Added to TheHive
        ↓
Slack Ticket Notification
```

## Ticket Information

A ticket may include:

- TheHive case number
- TheHive case ID
- Case title
- Severity
- Affected asset
- Requested action
- Reason for the handoff

## Duplicate Prevention

After creating a ticket, Shuffle adds a ticket-created tag to the TheHive case.

This prevents repeated case-update events from creating duplicate tickets.

## Ticket Closure Handback

```text
Zammad Ticket Closed
        ↓
Shuffle Receives Closure Event
        ↓
Linked TheHive Case Identified
        ↓
Completion Comment and Tag Added
        ↓
Slack Ticket and Case Notifications
        ↓
Tier 2 Validation
```

Closing a Zammad ticket does not automatically close the linked TheHive case.

Tier 2 must review and validate the completed work.

## Validation

The Detection Review path was validated by:

- Creating a Zammad ticket
- Linking it to a TheHive case
- Recording the ticket reference in TheHive
- Closing the ticket
- Synchronizing the result back to TheHive
- Sending Slack notifications

See:

- [02 – Case Updates and Ticket Routing](../../03-SOAR-Automation/02-Case-Updates-and-Ticket-Routing/)
- [03 – Ticket Closure Handback](../../03-SOAR-Automation/03-Ticket-Closure-Handback/)

## Test Data Notice

The ticket contents, assignments, comments, and completion messages shown in the repository are controlled test payloads used to validate the automation.

## Security Notes

- Zammad API tokens are not published.
- Ticket closure does not confirm that remediation was successful.
- Case linkage depends on consistent TheHive case information.
