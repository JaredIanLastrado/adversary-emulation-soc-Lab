# Case Updates and Ticket Routing – Validation Evidence

This directory contains evidence demonstrating successful execution of the AESOC Case Updates and Ticket Routing playbook.

The documented test used TheHive Case `17`. The scenario assumes that Tier 2 completed the investigation and requested a Detection Engineering review. The evidence validates the automation and ticket-routing process rather than the underlying investigation or detection review.

## Evidence Files

| File | What it demonstrates |
|---|---|
| [`01-TheHive-Case-Details.png`](01-TheHive-Case-Details.png) | TheHive case details, workflow tags, ownership, and detection-review request |
| [`02-Slack-Case-Created-Notification.png`](02-Slack-Case-Created-Notification.png) | The AESOC SOAR Agent delivered the case-created notification to Slack |
| [`03-Zammad-Detection-Review-Ticket.png`](03-Zammad-Detection-Review-Ticket.png) | Shuffle created a Detection Engineering ticket in Zammad and linked it to TheHive Case 17 |
| [`04-TheHive-Detection-Ticket-Comment.png`](04-TheHive-Detection-Ticket-Comment.png) | Shuffle added the Zammad ticket number and title to the originating TheHive case |
| [`05-Slack-Detection-Ticket-Notification.png`](05-Slack-Detection-Ticket-Notification.png) | The AESOC SOAR Agent delivered the Detection Engineering ticket-created notification |
| [`06-Slack-Case-Closed-Notification.png`](06-Slack-Case-Closed-Notification.png) | Shuffle detected the TheHive case closure and delivered the final case notification |

## Validated Execution Path

```text
TheHive Case Created
        ↓
Slack Case-Created Notification
        ↓
Tier 2 Requests Detection Review
        ↓
Shuffle Matches Detection Review Conditions
        ↓
Zammad Detection Engineering Ticket Created
        ↓
Ticket Reference Added to TheHive
        ↓
Slack Ticket-Created Notification
        ↓
TheHive Case Closed
        ↓
Slack Case-Closed Notification
```

## Validation Result

The evidence confirms that the playbook successfully:

- Received TheHive case events.
- Sent case-created and case-closed Slack notifications.
- Identified the Detection Engineering routing request.
- Created the corresponding Zammad ticket.
- Linked the Zammad ticket back to the originating TheHive case.
- Added a ticket-created tag to prevent duplicate routing.
- Sent the final Detection Engineering ticket notification.

**Overall validation result: Passed**

> This evidence validates the SOAR automation workflow only. A complete investigation report and Detection Engineering review were not produced for this test scenario.
