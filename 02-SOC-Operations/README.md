# 02 – SOC Operations

## Overview

This section documents the operational processes used to move AESOC alerts from initial detection through triage, investigation, response, external action, validation, and final closure.

The operating model connects:

- Wazuh alert generation
- Shuffle SOAR automation
- TheHive alert and case management
- Zammad action tracking
- Slack operational notifications
- Tier 1 alert triage
- Tier 2 investigation and response
- Detection Engineering review
- Infrastructure remediation
- Final case validation and closure

TheHive remains the primary investigation record. Zammad tracks work assigned outside the immediate SOC investigation, while Slack provides operational visibility.

> **Lab scope:** AESOC is a controlled home-lab environment. The documented roles represent separate operational responsibilities, although one individual may perform multiple roles during testing.

---

# Alert-to-Resolution Operational Workflow

[Open the standalone workflow](Alert-to-Resolution-Operational-Workflow.md)

## Lifecycle Summary

```text
Wazuh Alert Generated
        ↓
SOAR Playbook 01 – Wazuh Alert Intake
        ↓
TheHive Alert + Slack Notification
        ↓
Tier 1 Alert Triage
   ┌────┴───────────────┐
   ↓                    ↓
Close Alert       Escalate to Case
                         ↓
                Tier 2 Investigation
             ┌───────────┼────────────┐
             ↓           ↓            ↓
      Direct SOC     Detection    Infrastructure
        Action        Review       Remediation
             ↓           ↓            ↓
       Validate       Zammad        Zammad
        Result         Ticket         Ticket
                          └─────┬──────┘
                                ↓
                   SOAR Playbook 03 – Handback
                                ↓
                         Tier 2 Validation
                       ┌────────┴────────┐
                       ↓                 ↓
               More Action Required   Close Case
                                             ↓
                                Slack Case-Closed Notification
```

---

## Stage 1 – Detection and Automated Alert Intake

### Owner

**Automation layer**

### Process

1. Wazuh generates a security alert.
2. Wazuh sends alert metadata to Shuffle.
3. Shuffle evaluates the Wazuh rule level.
4. Shuffle creates a corresponding alert in TheHive.
5. Shuffle posts a notification to `#aesoc-alerts`.

### Severity routing

| Wazuh rule level | Classification |
|---:|---|
| 7–11 | Medium |
| 12–14 | High |
| 15 or higher | Critical |

### Responsibility endpoint

The automation stage is complete when:

- The alert exists in TheHive.
- The relevant metadata was transferred.
- The Slack alert notification was delivered.
- The alert is available for Tier 1 review.

---

## Stage 2 – Tier 1 Alert Triage

### Owner

**Tier 1 SOC Analyst**

### Responsibilities

Tier 1:

- Confirms that the alert contains sufficient context.
- Reviews the affected host, user, process, and event details.
- Checks for related or repeated activity.
- Determines whether the activity is expected, benign, suspicious, or likely malicious.
- Documents the triage rationale.
- Closes the alert or promotes it to a TheHive case.

### Outcomes

```text
Alert Reviewed
   ┌────┴──────────────┐
   ↓                   ↓
Close Alert       Escalate to Case
```

### Close alert

The alert may be closed when:

- The activity is expected or authorized.
- The alert is a confirmed false positive.
- Available evidence does not justify escalation.
- The closure rationale is documented.

### Escalate to case

The alert should be promoted to a case when:

- The activity may be malicious.
- Additional evidence collection is required.
- The scope or impact is unknown.
- Containment or remediation may be required.
- The alert is part of a broader pattern.
- Tier 2 investigation is required.

### Handoff endpoint

The Tier 1-to-Tier 2 handoff is complete when:

- A TheHive case has been created.
- The case is assigned to Tier 2.
- Tier 1 findings are documented.
- Relevant alert context and evidence are attached or linked.
- The reason for escalation is clear.

---

## Stage 3 – Tier 2 Investigation and Response

### Owner

**Tier 2 SOC Analyst**

### Responsibilities

Tier 2:

- Reviews the Tier 1 triage findings.
- Builds an investigation timeline.
- Determines the affected hosts, users, accounts, and systems.
- Collects supporting endpoint and network evidence.
- Determines whether the activity is benign, suspicious, or malicious.
- Assesses impact and remaining risk.
- Selects the appropriate response path.

### Response paths

| Response path | When it is used |
|---|---|
| Direct SOC Action | The required action can be completed through available SOC or endpoint-security tools |
| Detection Review | Detection logic, alert quality, or rule behavior requires review |
| Infrastructure Remediation | A system, network, firewall, patching, account, or other infrastructure action is required |
| Additional Investigation | Existing evidence is insufficient or the scope continues to expand |
| Final Closure | Investigation and required actions are complete and validated |

---

## Path A – Direct SOC Action

### Owner

**Tier 2 SOC Analyst**

Direct SOC actions may include:

- Endpoint isolation
- Process termination
- File quarantine or removal
- Account disablement when authorized
- Active-response actions through Wazuh
- Additional evidence collection
- Validation that suspicious activity has stopped

A Zammad ticket is not required when the SOC can complete and validate the action directly.

### Responsibility endpoint

The path is complete when:

- The action was executed.
- The result was validated.
- Evidence and findings were documented in TheHive.
- Remaining risk was evaluated.

---

## Path B – Detection Review

### Investigation owner

**Tier 2 SOC Analyst**

### Work-item owner

**Detection Engineering**

Tier 2 requests a Detection Engineering review when:

- A rule appears too noisy.
- Expected administrative behavior causes repeated alerts.
- A detection gap is identified.
- Detection severity may be inaccurate.
- Rule conditions require review.
- Additional detection coverage may be needed.

Tier 2 adds the required workflow tag to the TheHive case.

Shuffle then:

1. Creates a Detection Engineering ticket in Zammad.
2. Includes the linked TheHive case information.
3. Adds a ticket reference or comment to TheHive.
4. Adds a ticket-created tag.
5. Posts a notification to `#aesoc-tickets`.

Detection Engineering completes the review and closes the Zammad ticket.

SOAR Playbook 03 then:

1. Receives the Zammad closure event.
2. Identifies the linked TheHive case.
3. Adds a completion comment to the case.
4. Adds the detection-review completion tag.
5. Notifies `#aesoc-tickets`.
6. Notifies `#aesoc-cases`.
7. Returns the case to Tier 2.

---

## Path C – Infrastructure Remediation

### Investigation owner

**Tier 2 SOC Analyst**

### Work-item owner

**Infrastructure Operations or IT/Remediation**

Infrastructure remediation is used when the required action cannot or should not be completed directly by the SOC.

Examples include:

- Firewall-rule changes
- System patching
- Network-port changes
- Physical cable removal
- Server reconfiguration
- Production service changes
- Account administration outside SOC authority
- Hardware replacement or recovery

Tier 2 adds the infrastructure-remediation request tag to the TheHive case.

Shuffle then:

1. Creates an Infrastructure Remediation ticket in Zammad.
2. Includes the linked TheHive case information.
3. Adds the ticket reference to TheHive.
4. Adds a ticket-created tag.
5. Posts a notification to `#aesoc-tickets`.

Infrastructure Operations completes the work and closes the ticket.

SOAR Playbook 03 synchronizes the completion status back to TheHive and returns the case to Tier 2 for validation.

---

## Stage 4 – Tier 2 Handback Review

### Owner

**Tier 2 SOC Analyst**

The Zammad ticket being closed does not automatically mean that the security case is resolved.

Tier 2 must:

1. Review the Zammad completion notes.
2. Review any attached evidence.
3. Copy important findings into TheHive when necessary.
4. Validate that the requested action occurred.
5. Confirm that malicious or suspicious activity has stopped.
6. Determine whether additional action is required.
7. Close the case only after validation is complete.

### Outcomes

```text
Completed Work Reviewed
        ↓
   ┌────┴───────────────┐
   ↓                    ↓
More Action        Final Case
Required             Closure
```

---

# Ownership and Handoffs

[Open the standalone ownership guide](Ownership-and-Handoffs.md)

## Ownership Matrix

| Lifecycle stage | Owner | Supporting system | Responsibility endpoint |
|---|---|---|---|
| Alert generation | Wazuh | Wazuh | Alert generated |
| Automated alert intake | Shuffle | TheHive and Slack | Alert delivered and notification sent |
| Initial triage | Tier 1 SOC Analyst | TheHive | Alert closed or case created |
| Deep investigation | Tier 2 SOC Analyst | TheHive, Wazuh, Security Onion | Scope, findings, and response path documented |
| Direct SOC response | Tier 2 SOC Analyst | Wazuh and SOC tools | Action executed and validated |
| Detection review | Detection Engineering | Zammad | Review completed and ticket closed |
| Infrastructure remediation | Infrastructure Operations | Zammad | Requested action completed and ticket closed |
| Handback synchronization | Shuffle | TheHive and Slack | Completion status returned to the case |
| Final validation | Tier 2 SOC Analyst | TheHive | Result validated |
| Case closure | Tier 2 SOC Analyst | TheHive | Final decision documented and case closed |

---

## Handoff Requirements

Every handoff should include:

- Source alert or case identifier
- Affected host, account, user, or service
- Severity and current classification
- Investigation summary
- Evidence already reviewed
- Reason for the handoff
- Requested action
- Current owner
- Expected completion condition
- Linked TheHive case information

A handoff is incomplete when the receiving team cannot determine:

- What happened
- Why the work was assigned
- What action is expected
- Which case the work belongs to
- How completion should be validated

---

## System-of-Record Responsibilities

| System | Responsibility |
|---|---|
| TheHive | Primary alert, investigation, case, evidence, and closure record |
| Zammad | Assigned Detection Engineering and Infrastructure Remediation work |
| Slack | Operational notifications and awareness |
| Shuffle | Automated routing and synchronization |
| Wazuh | Host-based detection and endpoint alert data |
| Security Onion | Network alert and traffic-investigation data |

---

# Tier 1 Alert Triage Runbook

[Open the standalone Tier 1 runbook](Tier-1-Alert-Triage-Runbook.md)

## Objective

Tier 1 determines whether a Wazuh alert can be closed or requires escalation to a Tier 2 investigation.

## Triage Procedure

### 1. Confirm alert details

Review:

- Alert title
- Wazuh rule ID
- Rule level
- Timestamp
- Affected agent
- Agent IP address
- User or account
- Process name
- Command line
- Parent process
- Source and destination information
- MITRE ATT&CK mapping

### 2. Validate the data source

Confirm:

- The event came from the expected endpoint.
- The timestamp is reasonable.
- The agent is active.
- The alert is not an obvious duplicate.
- Required fields are present.
- The activity exists in the original Wazuh event.

### 3. Review context

Determine:

- Whether the user or process is expected.
- Whether the activity occurred during approved administration.
- Whether similar alerts occurred on the same host.
- Whether the activity is isolated or recurring.
- Whether related authentication, process, registry, or network events exist.

### 4. Assign an initial classification

| Classification | Meaning |
|---|---|
| Benign Expected | Authorized activity behaving as expected |
| Benign Administrative | Legitimate administrative activity that triggered a detection |
| False Positive | Detection fired incorrectly or without meaningful security value |
| Suspicious | Activity requires additional investigation |
| True Positive | Evidence supports malicious or unauthorized activity |

### 5. Select the outcome

#### Close the alert

Close when:

- The activity is understood.
- No additional investigation is required.
- The rationale is documented.
- Supporting context is included.

#### Escalate to Tier 2

Escalate when:

- Malicious intent is possible.
- Impact is unclear.
- More evidence is required.
- Multiple hosts or accounts may be involved.
- Containment may be required.
- The activity maps to a meaningful attack technique.
- The analyst cannot confidently close the alert.

---

## Minimum Tier 1 Notes

```text
Alert:
Affected Host:
Affected User:
Wazuh Rule:
Observed Activity:
Context Reviewed:
Related Activity:
Initial Classification:
Reason for Closure or Escalation:
Recommended Next Step:
```

---

## Tier 1 Completion Checklist

- [ ] Alert title and rule reviewed
- [ ] Affected asset confirmed
- [ ] User and process context reviewed
- [ ] Related alerts checked
- [ ] Original event validated
- [ ] Initial classification selected
- [ ] Triage rationale documented
- [ ] Alert closed or case created
- [ ] Tier 2 received sufficient context when escalated

---

# Tier 2 Investigation and Response Runbook

[Open the standalone Tier 2 runbook](Tier-2-Investigation-and-Response-Runbook.md)

## Objective

Tier 2 determines the scope, impact, classification, and required response for alerts escalated into TheHive cases.

## Investigation Procedure

### 1. Accept the handoff

Confirm:

- The correct case was assigned.
- Tier 1 notes are present.
- The alert and case identifiers match.
- The affected asset is identified.
- The reason for escalation is clear.

### 2. Establish the investigation question

Examples:

- Was the process execution authorized?
- Was the account activity expected?
- Did the activity spread to another host?
- Was persistence established?
- Was the endpoint compromised?
- Did the activity produce network indicators?
- Does the detection require tuning?

### 3. Build the timeline

Document important events in chronological order:

| Time | Event | Source | Significance |
|---|---|---|---|
| Timestamp | Initial activity | Wazuh or Security Onion | Starting point |
| Timestamp | Related event | Relevant log source | Supporting context |
| Timestamp | Response action | SOC or external team | Containment or remediation |
| Timestamp | Validation event | Monitoring platform | Confirms result |

### 4. Determine scope

Review:

- Affected hosts
- Users and accounts
- Processes and command lines
- Parent-child process relationships
- File hashes
- Registry activity
- Authentication events
- Network connections
- Related alerts
- Similar activity on other systems

### 5. Collect evidence

Evidence may include:

- Wazuh alert details
- Original Sysmon or Windows events
- Linux auditd or syslog records
- Security Onion alerts
- Zeek connection data
- Suricata alerts
- Packet or session evidence
- TheHive tasks and comments
- Zammad ticket results
- Screenshots and timestamps

### 6. Determine classification

| Classification | Meaning |
|---|---|
| Benign Expected | Approved and normal activity |
| Benign Administrative | Legitimate administrative behavior |
| False Positive | Detection logic produced an incorrect or low-value alert |
| Suspicious | Insufficient evidence for a final malicious determination |
| True Positive | Confirmed malicious or unauthorized behavior |

### 7. Select the response path

#### Direct SOC Action

Use when Tier 2 can complete the required action through available tools.

#### Detection Review

Use when the detection logic, severity, coverage, or alert quality requires review.

#### Infrastructure Remediation

Use when another team must change infrastructure, systems, networks, or services.

#### Additional Investigation

Use when evidence is incomplete or the incident scope continues to expand.

#### Closure

Use when findings and required actions have been validated.

---

## Tier 2 Investigation Notes

```text
Case:
Investigation Question:
Affected Assets:
Affected Users:
Timeline:
Evidence Reviewed:
Scope:
Impact:
Classification:
Containment or Remediation:
Detection Review Required:
Infrastructure Action Required:
Validation Performed:
Remaining Risk:
Final Decision:
```

---

## Tier 2 Completion Checklist

- [ ] Tier 1 handoff reviewed
- [ ] Investigation question defined
- [ ] Timeline documented
- [ ] Affected scope identified
- [ ] Supporting evidence collected
- [ ] Classification documented
- [ ] Impact evaluated
- [ ] Required response path selected
- [ ] External tickets linked when applicable
- [ ] Completed work validated
- [ ] Remaining risk evaluated
- [ ] Final decision documented

---

# Case Closure Checklist

[Open the standalone closure checklist](Case-Closure-Checklist.md)

## Investigation Completion

- [ ] Alert and case identifiers are correct
- [ ] Final classification is documented
- [ ] Investigation scope is documented
- [ ] Affected hosts, users, accounts, and services are identified
- [ ] Timeline is complete
- [ ] Relevant evidence is attached or referenced
- [ ] Analyst findings are written clearly
- [ ] MITRE ATT&CK mapping is included when applicable

## Containment and Remediation

- [ ] Required containment actions were completed
- [ ] Direct SOC actions were validated
- [ ] Detection Engineering results were reviewed
- [ ] Infrastructure Remediation results were reviewed
- [ ] Linked Zammad tickets are closed or otherwise resolved
- [ ] Important ticket findings were copied or summarized in TheHive
- [ ] No required action remains unassigned

## Validation

- [ ] The affected host or account was rechecked
- [ ] Continued suspicious activity was evaluated
- [ ] Detection behavior was retested when applicable
- [ ] Infrastructure changes were confirmed when applicable
- [ ] Remaining risk is understood
- [ ] Additional investigation is not required

## Closure Documentation

- [ ] Final disposition is selected
- [ ] Closure rationale is documented
- [ ] Containment or remediation outcome is recorded
- [ ] Detection outcome is recorded
- [ ] Lessons learned or tuning opportunities are noted
- [ ] Case owner is identified
- [ ] TheHive case status and stage are set to Closed
- [ ] Final case-closed notification was delivered

## Closure Authority

The Tier 2 SOC Analyst owns the final case-closure decision.

A closed Zammad ticket does not automatically authorize closure of the TheHive case. Tier 2 must review and validate the completed work before closing the investigation.

---

# Related SOAR Automation

| Playbook | Operational purpose |
|---|---|
| [01 – Wazuh Alert Intake](../03-SOAR-Automation/01-Wazuh-Alert-Intake/) | Creates TheHive alerts and sends severity-based Slack notifications |
| [02 – Case Updates and Ticket Routing](../03-SOAR-Automation/02-Case-Updates-and-Ticket-Routing/) | Sends case notifications and creates Detection Engineering or Infrastructure Remediation tickets |
| [03 – Ticket Closure Handback](../03-SOAR-Automation/03-Ticket-Closure-Handback/) | Synchronizes closed Zammad work back to TheHive and returns the case to Tier 2 |

---

# Operational Design Summary

The AESOC operating model separates the major responsibilities:

- **Automation** delivers and routes alerts and workflow events.
- **Tier 1** validates alerts and determines whether escalation is required.
- **Tier 2** owns the investigation, response decision, validation, and case closure.
- **Detection Engineering** reviews detection quality and rule behavior.
- **Infrastructure Operations** completes required system or network changes.
- **TheHive** remains the primary investigation record.
- **Zammad** tracks assigned external work.
- **Slack** provides lifecycle visibility.

This structure demonstrates the full alert-to-resolution lifecycle while keeping ownership, handoffs, and closure authority clearly defined.
