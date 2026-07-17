# Tier 1 Alert Triage Runbook

## Purpose

This runbook guides the initial review of Wazuh alerts delivered to TheHive through Shuffle.

Tier 1 determines whether an alert can be closed or must be escalated to Tier 2 as a case.

---

## Inputs

Tier 1 may receive:

- Wazuh rule ID and description
- Wazuh rule level
- Alert timestamp
- Affected agent
- Agent IP address
- User or account information
- Process and command-line details
- Parent-process information
- Source and destination information
- MITRE ATT&CK mapping
- Slack notification
- TheHive alert record

---

## Triage Workflow

```text
Receive Alert
      ↓
Validate Alert and Source
      ↓
Review Host, User, and Process Context
      ↓
Search for Related Activity
      ↓
Assign Initial Classification
      ↓
Close Alert or Escalate to Tier 2
```

---

## Step 1 – Confirm Alert Details

Review:

- Alert title
- Rule ID
- Rule level
- Timestamp
- Affected host
- Agent IP
- User
- Process
- Command line
- Parent process
- Event source
- MITRE ATT&CK mapping

Confirm that the alert and TheHive record refer to the same event.

---

## Step 2 – Validate the Event Source

Confirm:

- The Wazuh agent is the expected asset.
- The timestamp is reasonable.
- The original event exists.
- The relevant fields were transferred correctly.
- The alert is not an obvious duplicate.
- The event source is appropriate for the rule.

---

## Step 3 – Review Context

Determine:

- Whether the user was expected to perform the activity.
- Whether the process is legitimate.
- Whether the command line is normal.
- Whether the parent-child process relationship is expected.
- Whether the activity occurred during approved administration.
- Whether the same activity occurred repeatedly.
- Whether similar alerts exist on other hosts.
- Whether related authentication or network activity exists.

---

## Step 4 – Initial Classification

| Classification | Description |
|---|---|
| Benign Expected | Normal and authorized behavior |
| Benign Administrative | Legitimate administrative activity |
| False Positive | Alert logic produced an incorrect or low-value result |
| Suspicious | Additional investigation is required |
| True Positive | Evidence supports unauthorized or malicious activity |

---

## Step 5 – Determine the Outcome

### Close the alert

Close when:

- The activity is understood.
- It is authorized or expected.
- No additional evidence is required.
- No containment or remediation is required.
- The closure rationale is documented.

### Escalate to Tier 2

Escalate when:

- The activity may be malicious.
- The scope is unclear.
- More evidence is needed.
- Multiple hosts or accounts may be involved.
- Persistence or lateral movement is possible.
- Containment may be required.
- The alert is part of a broader pattern.
- The analyst cannot confidently close it.

---

## Minimum Triage Documentation

```text
Alert Title:
Wazuh Rule:
Affected Host:
Affected User:
Observed Activity:
Context Reviewed:
Related Alerts or Events:
Initial Classification:
Reason for Closure or Escalation:
Recommended Next Step:
```

---

## Escalation Handoff

Before escalating:

- Create or promote the alert to a TheHive case.
- Assign the case to Tier 2.
- Include the Tier 1 findings.
- Include related alert references.
- Attach or link supporting evidence.
- State why deeper investigation is required.

---

## Tier 1 Completion Checklist

- [ ] Alert title and rule reviewed
- [ ] Affected endpoint confirmed
- [ ] User and process context reviewed
- [ ] Original event validated
- [ ] Related alerts checked
- [ ] Initial classification selected
- [ ] Triage rationale documented
- [ ] Alert closed or case created
- [ ] Tier 2 received sufficient context when escalated

---

## Responsibility Endpoint

Tier 1 responsibility ends when either:

1. The alert is closed with a documented rationale, or
2. A complete and assigned TheHive case is handed to Tier 2.
