# Bridge Contract

## Purpose

The bridge contract defines how `nebulus-financial` declares which `nebulus-landscape` concepts it implements. It is the data source that allows `guiderail` to distinguish "modeled" from "realized" entities.

## Status

The bridge contract format is **TBD** — it will be defined when `nebulus-financial` begins development. This document captures the concept, the minimum required semantics, and the candidate approaches so the problem is named and bounded before implementation begins.

## Minimum required semantics

Regardless of format, the bridge contract must be able to answer:

1. **Which landscape IDs are realized in financial?**
   The contract must reference canonical landscape IDs (e.g., `cap.customer_onboarding`, `proc.account_opening_v1`).

2. **Where are they realized?**
   The contract must point to the location in financial — code path, module, service, or API.

3. **At what granularity is the realization claimed?**
   A capability might be fully realized, partially realized, or approximately realized. The contract should express this.

4. **Are there gaps or approximations worth noting?**
   Optional notes on what differs between the landscape model and the financial implementation.

## Candidate approaches

### Manifest file in financial
A dedicated file (e.g., `landscape-bridge.yaml`) in the `nebulus-financial` repo that maps landscape IDs to code paths.

```yaml
# Example shape (TBD)
realizations:
  - landscape_id: cap.customer_onboarding
    location: src/services/onboarding/
    granularity: full
  - landscape_id: proc.account_opening_v1
    location: src/workflows/account-opening.ts
    granularity: partial
    notes: manual review step not yet implemented
```

### Annotations in source code
Landscape IDs referenced in comments or metadata decorators within financial source code.

### Shared mapping file
A mapping file in a known location (possibly in landscape or a shared config repo) that both financial and guiderail consume.

## Architectural boundary

Landscape remains the canonical source of modeled business truth. The bridge contract does **not** redefine that truth — it only declares realization linkage into financial.

The bridge contract is consumed by guiderail to compute realization status. See `realization-status.md` for the lifecycle vocabulary.
