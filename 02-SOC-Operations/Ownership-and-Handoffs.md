# Ownership and Handoffs

## Purpose

This document defines who owns each stage of the AESOC alert lifecycle, when ownership changes, what information must accompany a handoff, and when a responsibility is considered complete.

> **Lab scope:** The roles are functional responsibilities. One individual may perform multiple roles during controlled home-lab testing.

---

## Role Summary

### Automation Layer

Responsible for:

- Receiving webhook events
- Routing alerts and case events
- Creating TheHive alerts
- Creating Zammad tickets
- Adding comments and tags
- Sending Slack notifications

Automation does not make the final investigation or closure decision.

### Tier 1 SOC Analyst

Responsible for:

- Initial alert review
- Context validation
- Initial classification
- Alert closure
- Escalation to Tier 2

### Tier 2 SOC Analyst

Responsible for:

- Deep investigation
- Scope and impact determination
- Evidence collection
- Response-path selection
- Direct SOC response
- External work requests
- Handback validation
- Final case closure

### Detection Engineering

Responsible for:

- Detection-quality review
- Rule-logic review
- Tuning recommendations
- Detection-gap analysis
- Validation of detection changes
- Closing the assigned Zammad work item

### Infrastructure Operations

Responsible for:

- System changes
- Network changes
- Firewall changes
- Patching
- Account or service administration
- Physical or production remediation
- Closing the assigned Zammad work item

---

## Ownership Matrix

| Stage | Owner | Record | Completion condition |
|---|---|---|---|
| Alert generation | Wazuh | Wazuh | Alert generated |
| Automated intake | Shuffle | TheHive and Slack | Alert and notification created |
| Initial triage | Tier 1 | TheHive | Alert closed or case escalated |
| Deep investigation | Tier 2 | TheHive | Findings and response path documented |
| Direct containment | Tier 2 | TheHive | Action completed and validated |
| Detection review | Detection Engineering | Zammad | Review completed and ticket closed |
| Infrastructure remediation | Infrastructure Operations | Zammad | Requested action completed and ticket closed |
| Automated handback | Shuffle | TheHive and Slack | Completion status synchronized |
| Final validation | Tier 2 | TheHive | Result reviewed and validated |
| Case closure | Tier 2 | TheHive | Final rationale recorded and case closed |

---

## Tier 1 to Tier 2 Handoff

### Trigger

Tier 1 determines that an alert requires deeper investigation.

### Required handoff information

- Alert title
- Alert identifier
- Affected endpoint
- Affected user or account
- Rule ID and severity
- Activity summary
- Evidence already reviewed
- Related alerts or events
- Initial classification
- Reason for escalation
- Recommended next step

### Responsibility endpoint

Tier 1 ownership ends when:

- TheHive case is created.
- Tier 1 notes are present.
- Relevant evidence is attached or linked.
- The case is assigned to Tier 2.

---

## Tier 2 to Detection Engineering Handoff

### Trigger

Tier 2 determines that detection logic requires review.

### Required handoff information

- TheHive case number and case ID
- Detection or rule being reviewed
- Reason for the request
- Expected behavior
- Observed behavior
- False-positive or detection-gap concern
- Supporting evidence
- Requested outcome
- Validation expectation

### Responsibility endpoint

Tier 2 retains ownership of the security case.

Detection Engineering owns only the assigned Zammad work item.

---

## Tier 2 to Infrastructure Operations Handoff

### Trigger

Tier 2 determines that another team must complete a system or infrastructure action.

### Required handoff information

- TheHive case number and case ID
- Affected system
- Requested action
- Reason for the action
- Urgency or priority
- Supporting evidence
- Expected completion state
- Validation requirement

### Responsibility endpoint

Tier 2 retains ownership of the security case.

Infrastructure Operations owns the assigned Zammad work item.

---

## Detection Engineering or Infrastructure Handback

### Trigger

The assigned Zammad ticket is moved to the closed state.

### Automated handback

Shuffle:

1. Receives the closure event.
2. Identifies the linked TheHive case.
3. Adds a completion comment.
4. Adds the appropriate completion tag.
5. Posts notifications to the ticket and case channels.

### Tier 2 responsibilities

Tier 2:

- Reviews the ticket result.
- Reviews comments and attachments.
- Copies important evidence into TheHive.
- Validates the technical result.
- Determines whether further action is required.

### Responsibility endpoint

The handback is complete when Tier 2 has reviewed and accepted the result.

---

## Systems of Record

| Information | Authoritative system |
|---|---|
| Alert details | TheHive and Wazuh |
| Investigation findings | TheHive |
| Investigation evidence | TheHive |
| Detection-review work | Zammad |
| Infrastructure-remediation work | Zammad |
| Case-closure decision | TheHive |
| Operational notifications | Slack |
| Workflow execution | Shuffle |

Slack notifications do not replace case notes, evidence, or ticket records.

---

## Handoff Quality Checklist

- [ ] Correct case and alert identifiers included
- [ ] Affected asset identified
- [ ] Reason for handoff explained
- [ ] Requested action stated clearly
- [ ] Supporting evidence included
- [ ] Current owner identified
- [ ] Receiving owner identified
- [ ] Completion condition defined
- [ ] Validation requirement defined
- [ ] TheHive and Zammad records linked
