# Tier 1 Triage

Tier 1 reviewed the raw Sysmon Event ID 10 telemetry and found 48 exact
matching alerts within 24 hours.

The repeated alerts shared the same:

- Endpoint
- Source process
- Target process
- Access mask
- OneDrive FileSync call trace

The observed access mask, `0x101411`, did not include
`PROCESS_VM_WRITE`, `PROCESS_VM_OPERATION`, or
`PROCESS_CREATE_THREAD`. No direct evidence of process injection or
endpoint compromise was identified.

No containment was required, but the repeated Level 12 alerts created a
detection-quality concern. The alert was promoted to TheHive Case 18
for Tier 2 review.

## Frequency Review

![Tier 1 alert-frequency review](Evidence/04-Wazuh-OneDrive-Alert-Frequency.png)

[View the repeatable frequency query](Tier-1-Frequency-Query.md)

## Triage Findings and Handoff

![Tier 1 investigation findings and handoff](Evidence/05-TheHive-Tier1-Triage-and-Handoff.png)

[Return to the full investigation](../)
