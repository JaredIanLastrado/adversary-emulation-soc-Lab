# Tier 2 Investigation

Tier 2 expanded the review across the AESOC environment, validated the
currently installed OneDrive executable, and examined the logic of
Wazuh Rule `92910`.

## Scope Review

All 48 Rule `92910` alerts during the review period matched the same
OneDrive-to-Explorer pattern.

No additional endpoint, source process, or process-access pattern was
identified.

![Environment-wide exact-pattern query](Evidence/06-Environment-Wide-Exact-Match.png)

![Total Rule 92910 volume](Evidence/07-Rule92910-Total-Volume.png)

![Tier 2 scope-review comment](Evidence/10-Tier2-Scope-Review-Comment.png)

## Binary Validation

The installed executable used the expected Microsoft OneDrive path and
metadata and had a valid Microsoft digital signature.

Because the binary had updated after the original event, the current
hash and version supported validation of the legitimate OneDrive
installation but were not treated as the exact historical binary.

![OneDrive binary validation](Evidence/08-OneDrive-Binary-Validation.png)

![Tier 2 binary-validation comment](Evidence/11-Tier2-Binary-Validation-Comment.png)

## Detection Logic Review

Rule `92910` generated a Level 12 alert for Sysmon Event ID 10 events
targeting `Explorer.exe`.

The rule did not evaluate:

- Source-process identity
- Source path
- Granted-access value
- Digital signature
- Call trace
- User context

Tier 2 classified the activity as benign application behavior and
requested formal Detection Engineering review because the broad rule
created repeated High-severity alert noise.

![Wazuh Rule 92910 logic](Evidence/09-Wazuh-Rule92910-Logic.png)

![Tier 2 detection-logic review](Evidence/12-Tier2-Detection-Logic-Review-Comment.png)

[Return to the full investigation](../)
