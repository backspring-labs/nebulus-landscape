# IDEA — Weaving Provider Research into the `nebulus-landscape` Canonical Model

## Purpose

Provider detail is important, but it should not become the organizing taxonomy of the landscape.

The `nebulus-landscape` model should remain:

- capability-first
- journey-aware
- process-aware
- risk/control-aware
- provider-enriched

That means provider research should be woven in as a **reference layer attached to the canonical model**, not used as the model’s top-level structure.

This document describes how to do that cleanly.

---

## Core Principle

The landscape should answer:

- what business domains exist?
- what capabilities matter?
- what journeys are taken?
- how are those journeys executed?
- what risks and controls exist?
- what systems and interfaces participate?

Provider research should then answer:

- where does this provider fit?
- what role do they play?
- which capabilities, journeys, and processes do they touch?
- what risks and controls do they affect?
- how confident are we in that mapping?

### Key rule

> Providers should be mapped into the landscape. They should not become the organizing structure of the landscape.

That rule should remain true across all future growth of the repo.

---

## Recommended Provider Layering

Provider material should be represented in three layers:

## 1. Provider Profile
The canonical provider identity layer.

This is the stable object that says:
- who the provider is
- what category they fit
- what broad role they appear to play

This is not where all research detail lives.

## 2. Provider Research
The rich evidence and interpretation layer.

This is where to keep:
- market notes
- product observations
- strengths and weaknesses
- implementation implications
- control/risk implications
- open questions
- source-backed findings

## 3. Provider Mapping
The explicit placement layer.

This is the bridge into the canonical model:
- capabilities touched
- journeys touched
- processes touched
- risks and controls affected
- systems/interfaces implied
- role in those contexts
- confidence in the mapping

This is the most important layer for product value.

---

## Recommended Folder Structure

```text
nebulus-landscape/
  docs/
    providers/
      alloy/
        provider.md
        research.md
        capabilities.md
        journeys.md
        processes.md
        risks-controls.md
      plaid/
        provider.md
        research.md
        capabilities.md
        journeys.md
        processes.md
        risks-controls.md

    capability-comparisons/
      customer-onboarding-providers.md
      identity-verification-providers.md
      fraud-decisioning-providers.md

    sources/
      provider-sources.md

  models/
    providers/
      prov.alloy.yaml
      prov.plaid.yaml

    provider-mappings/
      map.prov.alloy.yaml
      map.prov.plaid.yaml

    provider-research/
      research.prov.alloy.yaml
      research.prov.plaid.yaml

    sources/
      src.provider.alloy.site.yaml
      src.provider.alloy.docs.yaml
      src.provider.alloy.demo_notes.yaml
```

This shape cleanly separates:
- human-readable docs
- structured canonical models
- mappings
- provenance

---

## Canonical Provider Object

The canonical provider object should stay fairly lightweight.

### Example

```yaml
id: prov.alloy
name: Alloy
category: orchestration_and_decisioning
summary: Identity, fraud, and onboarding orchestration provider.
status: active_research
source_refs:
  - src.provider.alloy.site
  - src.provider.alloy.notes
tags:
  - onboarding
  - identity
  - fraud
```

### Why keep this light

The canonical provider object should be:
- stable
- referenceable
- easy to link to

It should not become a dumping ground for every opinion or observation.

That richer detail belongs in provider research artifacts.

---

## Provider Mapping Object

The provider mapping object is where the provider gets woven into the landscape.

### Example

```yaml
id: map.prov.alloy
provider_id: prov.alloy
capability_ids:
  - cap.customer_onboarding
  - cap.identity_verification
  - cap.fraud_decisioning
journey_ids:
  - journey.open_savings_account
process_ids:
  - proc.account_opening_v1
risk_ids:
  - risk.synthetic_identity
control_ids:
  - ctrl.idv_before_funding
system_ids:
  - sys.identity_orchestration
roles:
  - orchestration
  - decisioning
  - verification
confidence: medium
source_refs:
  - src.provider.alloy.notes
  - src.provider.alloy.docs
```

### Why this matters

This is the object that lets you do useful things later, such as:
- show providers in context in GuideRail/Viewscape
- generate provider comparisons within a capability
- trace provider participation through a journey or process
- connect providers to risks and controls
- reason about confidence and ambiguity explicitly

This is the main bridge into the rest of the model.

---

## Provider Research Object

The provider research object is where richer interpretation lives.

### Example

```yaml
id: research.prov.alloy
provider_id: prov.alloy
positioning_summary: >
  Strong fit in identity and fraud orchestration during onboarding flows.
strengths:
  - broad data-source orchestration
  - configurable workflow decisioning
  - strong onboarding fit
constraints:
  - may become central dependency in onboarding flow design
  - integration shape may vary by institution architecture
open_questions:
  - how configurable are downstream decision explanations?
  - what evidence surfaces are available for control/audit mapping?
source_refs:
  - src.provider.alloy.site
  - src.provider.alloy.demo_notes
  - src.provider.alloy.docs
last_reviewed: 2026-03-24
```

### Why this matters

This gives you a place for:
- interpretation
- uncertainty
- research notes
- comparative thinking

without contaminating the cleaner canonical object model.

---

## Markdown Docs per Provider

For each provider, it is useful to keep a readable document set.

## `provider.md`
This should cover:
- who the provider is
- how they position themselves
- what category they fit
- what they appear to do
- what they do not do
- broad role in the landscape

## `research.md`
This should cover:
- offering summary
- strengths
- weaknesses
- implementation observations
- risk/control implications
- open questions
- source inventory

## `capabilities.md`
This should cover:
- capabilities touched
- role per capability
- confidence
- where capability overlap is ambiguous

## `journeys.md`
This should cover:
- journeys touched
- where they enter the flow
- where they do not appear
- what actor-facing implications they have

## `processes.md`
This should cover:
- process participation by stage
- decision points touched
- control points touched
- operational implications

## `risks-controls.md`
This should cover:
- risks influenced
- controls enabled or supported
- evidence and auditability implications
- dependencies introduced by provider participation

This gives you both:
- human-readable reasoning
- structured model alignment

---

## Weaving Providers into Domain, Capability, Journey, and Process Artifacts

Providers should appear at the right depth, not everywhere equally.

## In Domain docs
Use a light touch:
- typical provider categories
- notable provider roles in the domain
- example provider classes

Do not let provider detail dominate the domain.

## In Capability docs
Use a stronger treatment:
- typical provider roles
- example providers
- why provider boundaries matter in this capability
- where overlap or ambiguity occurs

This is one of the best places to compare providers.

## In Journey docs
Use:
- typical systems/providers involved
- provider participation by stage
- which provider involvement is visible vs invisible to the user

This keeps provider detail tied to a real path.

## In Process docs
Use:
- provider participation by stage/task
- decision/control touchpoints
- evidence and audit implications
- integration dependencies

This is where provider detail becomes operationally meaningful.

## In Risk/Control docs
Use:
- provider-supported controls
- provider dependency implications
- evidence and auditability questions
- where provider placement affects control confidence

This makes provider placement governance-aware rather than purely commercial.

---

## Confidence and Provenance

Provider mappings should always carry:
- confidence
- source_refs
- open questions where needed

Why:
- providers often blur across capabilities
- marketing materials often overstate scope
- provider placement may be partially inferred
- different institutions may use the same provider in different roles

### Recommended confidence values
- high
- medium
- low
- candidate
- disputed

### Why this matters
This lets the model stay honest and keeps future users from assuming every provider placement is equally settled.

---

## Source Catalog Structure

You should track provider sources explicitly.

### Example

```yaml
id: src.provider.alloy.docs
type: provider_docs
title: Alloy documentation
provider_id: prov.alloy
notes: Primary source for product behavior and integration observations
status: active
```

### Useful source types
- provider_site
- provider_docs
- demo_notes
- analyst_notes
- comparison_notes
- implementation_observation
- internal_interpretation

This keeps provenance clean and queryable.

---

## Capability-Centric Provider Comparisons

One of the most useful artifacts will be capability-based provider comparison docs.

### Suggested structure

```text
docs/capability-comparisons/
  customer-onboarding-providers.md
  identity-verification-providers.md
  fraud-decisioning-providers.md
```

### Why this is powerful

Comparing providers abstractly is often weak.

Comparing them **within a capability context** is much more useful.

For example:
- how do providers differ in customer onboarding?
- how do they change the journey?
- what risk/control implications differ?
- what systems/interfaces do they imply?

That creates a much more grounded comparison surface.

---

## Recommended Research Prompt

Use this kind of prompt for each provider:

```text
You are helping populate the canonical repo nebulus-landscape.

Provider: [PROVIDER NAME]
Primary domain/capability context: [CAPABILITY OR DOMAIN]

Goal:
Research and place this provider into the canonical landscape model without letting the provider define the model.

Produce:
1. provider summary
2. capabilities touched
3. journeys touched
4. processes touched
5. risks/controls affected
6. role by capability/process
7. systems/interfaces implied
8. strengths
9. constraints
10. open questions
11. confidence assessment
12. provenance/source notes

Rules:
- capability-first, not vendor-first
- be explicit where the provider participates vs where it does not
- separate confidence from speculation
- do not assume the provider owns the whole flow
- note ambiguity honestly
```

---

## Best Working Pattern

For each provider, create:

### 1. Provider profile
The stable object and summary.

### 2. Provider research
The richer evidence-backed notes.

### 3. Provider mapping
The structured placement into:
- capabilities
- journeys
- processes
- risks
- controls
- systems

### 4. Capability comparison references
Compare multiple providers inside a capability context.

This gives you:
- clean canonical identity
- rich research
- useful model placement
- practical comparison

---

## Relationship to the Canonical Model

The key relationship should be:

- `nebulus-landscape` remains capability-first and process-aware
- provider research enriches and references that model
- providers are placed into the landscape through mappings
- the business architecture stays primary

### Strong design rule

> Providers should be mapped into capabilities, journeys, processes, risks, controls, and systems. They should not become the organizing taxonomy of the landscape.

That rule should remain explicit in the repo principles.

---

## Best One-Line Summary

> Weave provider detail into `nebulus-landscape` as a mapped reference layer: canonical provider objects, research artifacts, and explicit mappings into capabilities, journeys, processes, risks, controls, and systems — while keeping the overall landscape capability-first rather than vendor-first.

---

## Final View

This approach gives you:
- a canonical landscape model that stays stable
- provider research that stays useful
- provenance and confidence that stay visible
- comparisons that stay grounded in real business capabilities
- a clean bridge into GuideRail/Viewscape later

That is the right long-term shape.
