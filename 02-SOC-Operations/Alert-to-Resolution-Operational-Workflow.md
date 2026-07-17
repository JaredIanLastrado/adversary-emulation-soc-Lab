# Alert-to-Resolution Operational Workflow

## Purpose

This workflow documents how an AESOC alert moves from Wazuh detection through automated intake, Tier 1 triage, Tier 2 investigation, response, external action, validation, and closure.

> **Lab scope:** The roles represent separate operational responsibilities within the simulated SOC. One person may perform multiple roles during controlled lab testing.

---

## Complete Lifecycle

```text
Wazuh Alert Generated
        ↓
SOAR Playbook 01 – Wazuh Alert Intake
        ↓
TheHive Alert Created
        ↓
Slack #aesoc-alerts Notification
        ↓
Tier 1 Alert Triage
   ┌────┴───────────────┐
   ↓                    ↓
Close Alert       Promote to Case
                         ↓
                  Tier 1 → Tier 2 Handoff
                         ↓
                Slack #aesoc-cases Notification
                         ↓
                 Tier 2 Investigation
            ┌────────────┼──────────────┐
            ↓            ↓              ↓
      Direct SOC     Detection     Infrastructure
        Action        Review        Remediation
            ↓            ↓              ↓
       Validate      Zammad Ticket   Zammad Ticket
        Result             └──────┬──────┘
                                  ↓
                      Assigned Team Completes Work
                                  ↓
                         Zammad Ticket Closed
                                  ↓
                    SOAR Playbook 03 – Handback
                                  ↓
                       TheHive Comment and Tag
                                  ↓
                      Slack Ticket and Case Update
                                  ↓
                           Tier 2 Validation
                        ┌─────────┴─────────┐
                        ↓                   ↓
                More Action Required   Final Closure
                                                ↓
                              Slack #aesoc-cases Notification
```

---

## Stage 1 – Wazuh Alert Generation

**Owner:** Wazuh

Wazuh evaluates endpoint telemetry and generates an alert when a configured rule matches.

### Output

- Alert title
- Rule ID
- Rule level
- Timestamp
- Affected endpoint
- Available process or user context
- MITRE ATT&CK metadata when present

### Responsibility endpoint

The alert has been generated and is available for automated intake.

---

## Stage 2 – Automated Alert Intake

**Owner:** Shuffle SOAR

SOAR Playbook 01:

1. Receives the Wazuh alert metadata.
2. Evaluates the Wazuh rule level.
3. Routes the alert through the appropriate severity branch.
4. Creates a TheHive alert.
5. Posts a notification to `#aesoc-alerts`.

### Responsibility endpoint

- TheHive alert created
- Slack alert notification delivered
- Alert available for Tier 1

---

## Stage 3 – Tier 1 Alert Triage

**Owner:** Tier 1 SOC Analyst

Tier 1:

- Validates the alert.
- Reviews endpoint, user, process, and event context.
- Searches for related activity.
- Assigns an initial classification.
- Documents the triage rationale.

### Decision

```text
Close the Alert
or
Promote the Alert to a Case
```

### Close-alert endpoint

The alert is closed with a documented rationale.

### Escalation endpoint

A TheHive case is created, assigned to Tier 2, and supplied with Tier 1 context.

---

## Stage 4 – Tier 2 Investigation

**Owner:** Tier 2 SOC Analyst

Tier 2:

- Reviews the handoff.
- Defines the investigation question.
- Builds the timeline.
- Determines scope and impact.
- Collects supporting evidence.
- Selects the response path.

### Response options

- Direct SOC Action
- Detection Review
- Infrastructure Remediation
- Additional Investigation
- Final Closure

---

## Direct SOC Action

Tier 2 completes the action using authorized SOC tools.

Examples include:

- Host isolation
- Process termination
- Evidence collection
- Active response
- File quarantine
- Validation of endpoint state

### Endpoint

The action and validation results are documented in TheHive.

---

## Detection Review

Tier 2 requests Detection Engineering support.

SOAR Playbook 02:

- Creates a Zammad ticket.
- Adds a ticket reference to TheHive.
- Adds a ticket-created tag.
- Notifies `#aesoc-tickets`.

Detection Engineering completes the review and closes the ticket.

SOAR Playbook 03:

- Receives the ticket closure.
- Updates the linked TheHive case.
- Adds the completion tag.
- Posts ticket and case notifications.
- Returns the case to Tier 2.

---

## Infrastructure Remediation

Tier 2 requests Infrastructure Operations support.

SOAR Playbook 02:

- Creates the Zammad remediation ticket.
- Adds the ticket reference to TheHive.
- Adds a ticket-created tag.
- Notifies `#aesoc-tickets`.

Infrastructure Operations completes the action and closes the ticket.

SOAR Playbook 03 synchronizes the completion state back to TheHive.

---

## Tier 2 Handback Review

Tier 2:

- Reviews ticket comments and attachments.
- Confirms the requested work was completed.
- Copies important evidence into TheHive.
- Validates the technical result.
- Checks for continued suspicious activity.
- Determines whether another action is required.

### Outcomes

```text
More Action Required
or
Final Case Closure
```

---

## Final Case Closure

**Owner and closure authority:** Tier 2 SOC Analyst

Tier 2:

- Documents the final decision.
- Records completed containment or remediation.
- Records detection-review results.
- Evaluates remaining risk.
- Sets the TheHive status and stage to Closed.

SOAR Playbook 02 posts the final case-closed notification to `#aesoc-cases`.

---

## Lifecycle Systems

| System | Lifecycle role |
|---|---|
| Wazuh | Detection and endpoint alert generation |
| Shuffle | Automated routing and synchronization |
| TheHive | Alert and investigation system of record |
| Zammad | Detection and remediation work tracking |
| Slack | Operational notifications |
| Security Onion | Network alert and investigation data |

---

## Related Documentation

- [Ownership and Handoffs](Ownership-and-Handoffs.md)
- [Tier 1 Alert Triage Runbook](Tier-1-Alert-Triage-Runbook.md)
- [Tier 2 Investigation and Response Runbook](Tier-2-Investigation-and-Response-Runbook.md)
- [Case Closure Checklist](Case-Closure-Checklist.md)
