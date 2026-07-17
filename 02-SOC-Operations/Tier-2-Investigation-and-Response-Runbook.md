# Tier 2 Investigation and Response Runbook

## Purpose

This runbook guides the investigation and response process for alerts escalated from Tier 1 into TheHive cases.

Tier 2 owns the investigation, response decision, validation, and final case closure.

---

## Investigation Workflow

```text
Accept Tier 1 Handoff
        ↓
Review Initial Findings
        ↓
Define Investigation Question
        ↓
Build Timeline
        ↓
Determine Scope and Impact
        ↓
Collect and Analyze Evidence
        ↓
Assign Final Classification
        ↓
Select Response Path
        ↓
Validate Results
        ↓
Close Case or Continue Investigation
```

---

## Step 1 – Accept the Handoff

Confirm:

- The correct case is assigned.
- Tier 1 notes are present.
- The escalation reason is clear.
- The affected host or user is identified.
- The alert and case identifiers match.
- Relevant evidence is linked.

Return the case for clarification when the handoff lacks sufficient context.

---

## Step 2 – Define the Investigation Question

Examples:

- Was the process execution authorized?
- Was the account activity expected?
- Did the activity establish persistence?
- Did the activity spread to another host?
- Was the endpoint compromised?
- Was sensitive access attempted?
- Does the detection need tuning?
- Is infrastructure remediation required?

---

## Step 3 – Build the Timeline

| Time | Event | Source | Significance |
|---|---|---|---|
| Timestamp | Initial alert activity | Wazuh | Investigation starting point |
| Timestamp | Related event | Wazuh or Security Onion | Supporting context |
| Timestamp | Analyst or response action | TheHive or SOC tool | Investigation or containment |
| Timestamp | External-team action | Zammad | Detection or remediation work |
| Timestamp | Validation activity | Monitoring platform | Confirms result |

---

## Step 4 – Determine Scope

Review:

- Affected hosts
- Affected users
- Accounts and privileges
- Processes and command lines
- Parent-child process chains
- File hashes
- Registry activity
- Scheduled tasks
- Authentication activity
- Network connections
- Related alerts
- Similar behavior on other endpoints
- Persistence indicators
- Lateral-movement indicators

---

## Step 5 – Collect Evidence

Evidence may include:

- Wazuh alert details
- Original Sysmon events
- Windows Event Logs
- Auditd records
- Syslog records
- Security Onion alerts
- Zeek connection records
- Suricata alerts
- Packet or session evidence
- Screenshots
- Hashes and file paths
- TheHive comments and tasks
- Zammad ticket comments and attachments

---

## Step 6 – Determine Classification

| Classification | Description |
|---|---|
| Benign Expected | Approved and normal behavior |
| Benign Administrative | Legitimate administrative activity |
| False Positive | Detection logic produced an incorrect alert |
| Suspicious | Evidence is concerning but inconclusive |
| True Positive | Confirmed malicious or unauthorized behavior |

---

## Step 7 – Select the Response Path

### Direct SOC Action

Use when the action can be completed and validated through authorized SOC tools.

Examples:

- Host isolation
- Process termination
- File quarantine
- Active response
- Additional evidence collection

### Detection Review

Use when:

- Detection logic may be noisy.
- Severity may be incorrect.
- A detection gap exists.
- A custom rule may be required.
- Rule behavior requires validation.

### Infrastructure Remediation

Use when:

- Firewall changes are required.
- Patching is required.
- System reconfiguration is required.
- Production or physical infrastructure must be changed.
- Another team owns the required action.

### Additional Investigation

Use when:

- Evidence remains incomplete.
- Scope continues to expand.
- New indicators are discovered.
- Another host or user becomes involved.

---

## Step 8 – Review External Work

When a linked Zammad ticket closes:

1. Review the completion notes.
2. Review comments and attachments.
3. Confirm that the correct case was updated.
4. Copy important findings into TheHive.
5. Validate the action.
6. Check for continued suspicious activity.
7. Determine whether more action is required.

A closed ticket does not automatically resolve the security case.

---

## Step 9 – Validate the Outcome

Validation may include:

- Rechecking the endpoint
- Confirming host isolation
- Confirming process termination
- Confirming file removal
- Confirming account disablement
- Confirming firewall or infrastructure changes
- Repeating a controlled detection test
- Confirming the alert no longer behaves incorrectly
- Reviewing new Wazuh or Security Onion activity

---

## Tier 2 Investigation Notes

```text
Case:
Investigation Question:
Affected Assets:
Affected Users or Accounts:
Timeline:
Evidence Reviewed:
Scope:
Impact:
Classification:
Containment Actions:
Detection Review:
Infrastructure Remediation:
Validation Performed:
Remaining Risk:
Final Decision:
```

---

## Tier 2 Completion Checklist

- [ ] Tier 1 handoff reviewed
- [ ] Investigation question defined
- [ ] Timeline documented
- [ ] Scope determined
- [ ] Supporting evidence collected
- [ ] Classification documented
- [ ] Impact evaluated
- [ ] Response path selected
- [ ] Direct actions documented
- [ ] Zammad tickets linked when applicable
- [ ] External-team results reviewed
- [ ] Technical validation completed
- [ ] Remaining risk evaluated
- [ ] Final decision documented

---

## Responsibility Endpoint

Tier 2 responsibility ends only when:

- The investigation is complete.
- Required actions are complete.
- Results are validated.
- Remaining risk is documented.
- The case-closure checklist is complete.
- TheHive is updated with the final decision.
