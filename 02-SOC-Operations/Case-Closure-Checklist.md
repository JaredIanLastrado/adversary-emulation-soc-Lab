# Case Closure Checklist

## Purpose

This checklist helps ensure that an AESOC case is not closed while investigation, containment, remediation, validation, or documentation remains incomplete.

The Tier 2 SOC Analyst owns the final closure decision.

---

## Investigation Completion

- [ ] Alert and case identifiers are correct
- [ ] Final classification is documented
- [ ] Investigation question was answered
- [ ] Investigation scope is documented
- [ ] Affected hosts are identified
- [ ] Affected users and accounts are identified
- [ ] Relevant services or applications are identified
- [ ] Timeline is complete
- [ ] Supporting evidence is attached or referenced
- [ ] Analyst findings are written clearly
- [ ] MITRE ATT&CK mapping is included when applicable
- [ ] Related alerts or cases were reviewed

---

## Containment and Response

- [ ] Required containment actions were completed
- [ ] Direct SOC actions are documented
- [ ] Endpoint state was validated
- [ ] Malicious processes or files were addressed
- [ ] Affected accounts were addressed when required
- [ ] Continued suspicious activity was checked
- [ ] No required containment action remains pending

---

## Detection Review

Complete when applicable:

- [ ] Detection-review request is documented
- [ ] Zammad Detection Engineering ticket is linked
- [ ] Detection Engineering completion notes were reviewed
- [ ] Detection outcome is summarized in TheHive
- [ ] Detection change was validated when applicable
- [ ] No-change decision was documented when applicable
- [ ] Remaining detection risk is understood

---

## Infrastructure Remediation

Complete when applicable:

- [ ] Infrastructure-remediation request is documented
- [ ] Zammad remediation ticket is linked
- [ ] Infrastructure completion notes were reviewed
- [ ] Required system or network change was confirmed
- [ ] Remediation evidence is included
- [ ] Important findings were copied into TheHive
- [ ] No infrastructure action remains pending

---

## External Tickets

- [ ] All required Zammad tickets are linked
- [ ] Ticket owners are identified
- [ ] Ticket status is confirmed
- [ ] Completion comments were reviewed
- [ ] Relevant attachments were reviewed
- [ ] Ticket results were validated by Tier 2
- [ ] Closed tickets do not require additional action

---

## Validation

- [ ] Affected host or account was rechecked
- [ ] Continued suspicious activity was evaluated
- [ ] Detection behavior was retested when applicable
- [ ] Infrastructure changes were verified when applicable
- [ ] Containment effectiveness was confirmed
- [ ] Remaining risk is understood
- [ ] Additional investigation is not required
- [ ] No unresolved task remains assigned

---

## Closure Documentation

- [ ] Final disposition is selected
- [ ] Closure rationale is documented
- [ ] Investigation summary is complete
- [ ] Containment outcome is recorded
- [ ] Remediation outcome is recorded
- [ ] Detection-review outcome is recorded
- [ ] Remaining risk is documented
- [ ] Lessons learned are noted when applicable
- [ ] Detection-tuning opportunities are noted
- [ ] Case owner is identified
- [ ] Closing analyst is identified

---

## TheHive Closure

- [ ] Required comments are present
- [ ] Required tags are present
- [ ] Important evidence is attached or referenced
- [ ] Linked Zammad tickets are documented
- [ ] Tasks are completed or resolved
- [ ] Final case status is selected
- [ ] Case stage is set to Closed
- [ ] Closure timestamp is recorded
- [ ] Final case-closed notification was delivered

---

## Closure Authority

A Zammad ticket being closed does not automatically authorize closure of the linked TheHive case.

The Tier 2 SOC Analyst must review the completed work, validate the result, evaluate remaining risk, and document the final decision before closing the case.

---

## Final Closure Statement

```text
Final Classification:
Affected Assets:
Investigation Summary:
Containment or Remediation Completed:
Detection Review Outcome:
Validation Performed:
Remaining Risk:
Closure Rationale:
Closed By:
Closure Time:
```
