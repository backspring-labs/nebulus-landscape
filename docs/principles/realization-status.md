# Realization Status

## Purpose

This document defines the lifecycle vocabulary that the triad uses to distinguish between modeled concepts in `nebulus-landscape` and realized implementations in `nebulus-financial`. This vocabulary is consumed by `guiderail` to render entities honestly based on their maturity.

## Lifecycle states

Every entity in the landscape can be in one of three states from guiderail's perspective:

### Modeled

Exists in `nebulus-landscape`, not yet implemented in `nebulus-financial`.

Guiderail can show the business architecture — domain, capability, journey, process, provider research — but there is no code to tour. This is authored research with provenance, not vapor. It is the business architecture equivalent of an API spec before implementation exists.

### Realized

Exists in `nebulus-landscape` AND implemented in `nebulus-financial`.

Guiderail can show both the concept and the code. This is the gold standard — the full guided tour from business truth through executable implementation.

### Implemented but unmodeled

Exists in `nebulus-financial` code but does not yet have a clean `nebulus-landscape` artifact.

Guiderail can still tour the code, but there is no canonical business architecture context to frame it. This is a valid **discovery state** but should generally trigger follow-up modeling work rather than remain the desired long-term condition. If financial implements something that landscape doesn't model, the triad is drifting and repair work is needed.

## Key rules

### Realization status is derived, not authored

In `nebulus-landscape`, authors do **not** manually maintain realization lifecycle state in canonical YAML. Guiderail derives it later from two-source ingestion:
- If a landscape entity has a matching bridge reference in financial → **realized**
- If a landscape entity has no bridge reference → **modeled**
- If a financial artifact has no landscape counterpart → **implemented but unmodeled**

This prevents a half-manual, half-derived lifecycle mess.

### Landscape remains the design authority

Realization status does not change the canonical truth of the landscape model. A "modeled" capability is still a real, authored, sourced business architecture artifact. Realization status only describes whether an executable counterpart exists — it does not grade the quality or importance of the landscape artifact itself.

### Two-source rendering is guiderail's responsibility

The logic for ingesting both landscape and financial, stitching them by ID, and rendering lifecycle states is a guiderail concern. It is not a landscape repo concern or a financial repo concern.

## Relationship to the bridge contract

The bridge contract (see `bridge-contract.md`) is the mechanism that declares which landscape concepts are realized in financial. Realization status is the vocabulary; the bridge contract is the data source. Without the bridge contract, guiderail cannot compute lifecycle states.
