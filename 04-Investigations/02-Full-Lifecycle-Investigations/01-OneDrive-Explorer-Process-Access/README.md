# OneDrive.exe Access to Explorer.exe Investigation

## Executive Summary

Wazuh generated a Level 12 alert after Microsoft OneDrive accessed
`Explorer.exe` through Sysmon Event ID 10.

The alert progressed through the complete AESOC alert-to-resolution
lifecycle:

```text
Wazuh
  ↓
Shuffle
  ↓
TheHive Alert
  ↓
Tier 1 Triage
  ↓
TheHive Case 18
  ↓
Tier 2 Investigation
  ↓
Detection Engineering
  ↓
Zammad Ticket 72057
  ↓
TheHive Closure Handback
  ↓
Final Case Closure
```

The activity was confirmed as legitimate Microsoft OneDrive behavior.
No evidence of process injection or endpoint compromise was identified.

Detection Engineering confirmed that Wazuh Rule `92910` was overly
broad. A proposed tuning rule passed controlled condition testing but
failed live validation. Detection tuning was therefore deferred while
the original detection coverage remained enabled.

## Final Outcome

| Field | Result |
|---|---|
| Classification | True Positive — Benign Application Behavior |
| Impact | No |
| Containment | Not required |
| Remediation | Not required |
| Detection tuning | Required but deferred |
| Existing coverage | Preserved |
| Case status | Closed |

---

## 1. Alert Intake

Wazuh Rule `92910` generated a Level 12 alert for a Sysmon Event ID 10
process-access event.

| Field | Value |
|---|---|
| Endpoint | `Win10Client-Agent` |
| Source process | `OneDrive.exe` |
| Target process | `Explorer.exe` |
| Granted access | `0x101411` |
| MITRE mapping | T1055 — Process Injection |

![Wazuh core process-access fields](00-Alert-Intake/02-Wazuh-ProcessAccess-Core-Fields.png)

The call trace contained expected OneDrive FileSync components:

- `FileSyncClient.dll`
- `FileSyncEvents.dll`
- `FileSyncHost.DLL`

![OneDrive process-access call trace](00-Alert-Intake/03-Wazuh-ProcessAccess-CallTrace.png)

The complete event is available as
[raw Wazuh alert JSON](00-Alert-Intake/01-Raw-Wazuh-Alert-1783532403.27005643.json).

---

## 2. Tier 1 Triage

Tier 1 reviewed the event and identified 48 exact matching alerts within
24 hours, occurring approximately once every 30 minutes.

The access mask did not include the permissions commonly associated
with direct memory writing or remote-thread creation:

- `PROCESS_VM_WRITE`
- `PROCESS_VM_OPERATION`
- `PROCESS_CREATE_THREAD`

![Tier 1 alert-frequency review](01-Tier-1-Triage/Evidence/04-Wazuh-OneDrive-Alert-Frequency.png)

Tier 1 assessed the activity as likely benign application behavior.
No containment was required, but the alert was promoted to TheHive
Case 18 for Tier 2 detection-quality review.

![Tier 1 disposition and Tier 2 handoff](01-Tier-1-Triage/Evidence/05-TheHive-Tier1-Triage-and-Handoff.png)

[View the Tier 1 deep dive](01-Tier-1-Triage/)

---

## 3. Tier 2 Investigation

Tier 2 expanded the query across the AESOC environment.

The exact OneDrive-to-Explorer pattern returned 48 alerts. A separate
query for all Rule `92910` alerts returned the same total, indicating
that all observed Rule `92910` activity during the review period came
from the same recurring pattern.

![Environment-wide exact-pattern query](02-Tier-2-Investigation/Evidence/06-Environment-Wide-Exact-Match.png)

![Total Rule 92910 volume](02-Tier-2-Investigation/Evidence/07-Rule92910-Total-Volume.png)

The currently installed OneDrive executable was validated as a
Microsoft-signed binary running from the expected path.

![OneDrive binary validation](02-Tier-2-Investigation/Evidence/08-OneDrive-Binary-Validation.png)

The installed executable had updated after the original alert. The
current signature, hash, version, and path therefore supported
validation of the legitimate OneDrive installation but were not treated
as definitive validation of the exact historical binary.

Rule `92910` was found to evaluate only Sysmon Event ID 10 events
targeting `Explorer.exe`. It did not evaluate source process identity,
access mask, executable trust, call trace, or user context.

![Wazuh Rule 92910 logic](02-Tier-2-Investigation/Evidence/09-Wazuh-Rule92910-Logic.png)

Tier 2 concluded that the activity was benign, but the repeated
High-severity alerts warranted formal Detection Engineering review.

[View the Tier 2 deep dive](02-Tier-2-Investigation/)

---

## 4. Detection Engineering

Detection Engineering accepted Zammad Ticket `#72057` and confirmed
that tuning was warranted.

![Detection review accepted](03-Detection-Engineering/Evidence/13-Zammad-Detection-Review-Accepted.png)

Custom Rule `100929` was proposed to downgrade only the validated
OneDrive pattern.

The proposed conditions required:

- Expected OneDrive installation path
- Granted access `0x101411`
- Expected FileSync DLLs in the call trace

Controlled testing produced the following results:

| Test | Result |
|---|---|
| Approved OneDrive pattern | Passed |
| Unexpected source path | Did not match, as expected |
| Different access mask | Did not match, as expected |
| Unexpected call trace | Did not match, as expected |

![Positive controlled-rule test](03-Detection-Engineering/Evidence/15-Controlled-Test-Positive-Phase3.png)

Live validation failed because new events continued to match Rule
`92910` at Level 12 instead of Rule `100929` at Level 3.

![Failed live validation](03-Detection-Engineering/Evidence/16-Live-Validation-Rule92910-Persisted.png)

No production detection change was retained. Detection tuning was
deferred because the proposed child rule did not supersede the original
rule during live Windows EventChannel processing.

![Detection review completed](03-Detection-Engineering/Evidence/17-Zammad-Detection-Review-Completed.png)

![Closed Detection Engineering ticket](03-Detection-Engineering/Evidence/18-Zammad-Detection-Ticket-Closed.png)

[View the Detection Engineering deep dive](03-Detection-Engineering/)

---

## 5. Final Disposition

After Zammad Ticket `#72057` closed, Shuffle synchronized the Detection
Engineering completion status back to TheHive for Tier 2 review.

![TheHive Detection Engineering closure handback](04-Final-Disposition/Evidence/20-TheHive-Detection-Review-Closure-Handback.png)

Tier 2 reviewed the completed Detection Engineering work and closed the
TheHive case.

Final determination:

- True Positive — Benign Application Behavior
- No security impact
- No containment required
- No remediation required
- Detection tuning deferred
- Existing detection coverage preserved

![TheHive final case closure](04-Final-Disposition/Evidence/19-TheHive-Case-Closed.png)

[View the final disposition](04-Final-Disposition/)

---

## Skills Demonstrated

- Alert triage
- Process-access analysis
- Wazuh and Sysmon investigation
- Scope and frequency analysis
- Binary and digital-signature validation
- Detection-logic review
- Detection Engineering handoff
- Controlled positive and negative rule testing
- Case and ticket lifecycle management
- Honest documentation of unsuccessful production validation
