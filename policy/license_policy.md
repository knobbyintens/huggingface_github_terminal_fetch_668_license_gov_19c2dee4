# Dataset License Review Policy

Version: 1.0

This policy governs every dataset that is mirrored, redistributed or used in a
product by the data platform group.  An audit compares the license recorded in
`registry/dataset_inventory.json` (the *reported* license) against the license
published on the dataset's public hub card (the *detected* license).

## Usage tiers

- `commercial` – the dataset feeds a customer-facing or revenue-generating
  product.
- `research_only` – the dataset is used for internal research and prototyping
  only.

## Decision rules (evaluate in order, first match wins)

1. **Unverifiable license** – if the detected license is missing, blank,
   `unknown`, `other` or otherwise not a concrete license identifier, the
   dataset must be escalated with reason `license_unverifiable`.
2. **Commercial use of a non-commercial license** – if the detected license
   forbids commercial use (for example any license containing the `NC`
   restriction, `research-only`, or `academic-only`) and the usage tier is
   `commercial`, the dataset must be escalated with reason
   `commercial_use_not_permitted`.
3. **Declared license mismatch** – if a concrete license is detected and it
   differs from the reported license, the entry is corrected: the inventory
   declaration is updated to the detected license and the result is recorded
   with reason `declared_license_mismatch`.
4. **Compliant** – otherwise the dataset is verified with reason `compliant`.

## Follow-up actions

- `escalated` results require a tracking issue titled
  `[License Review] <dataset_id>`.
- `corrected` results require a tracking issue titled
  `[License Correction] <dataset_id>`.
- `verified` results require no issue.

Every issue body must contain, one per line:

```
Dataset: <dataset_id>
Reason: <reason code>
Reported license: <reported license>
Verified license: <detected license>
Usage tier: <usage_tier>
```

## Registry bookkeeping

After the audit each inventory entry must carry:

- `status` – `verified`, `corrected` or `escalated`
- `reason` – one of the reason codes above
- `verified_license` – the detected license identifier
- `last_audited` – the audit date (`YYYY-MM-DD`)

For `corrected` entries the `declared_license` field is rewritten to the
detected value.  All other descriptive fields stay untouched.
