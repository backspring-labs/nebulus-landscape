# IDEA — `nebulus-landscape` Canonical Model

## Purpose

`nebulus-landscape` should not be a loose note repository, a vendor catalog, or a folder full of disconnected industry thoughts.

It should be a **canonical knowledge model** for the fintech and banking landscape that is:

- business-architecture-first
- capability-centered
- journey-aware
- process-aware
- risk/control-aware
- provider-enriched
- linkable to executable reference implementations such as `nebulus-financial`

The purpose of the repo is to create a durable substrate for:

- research
- strategy
- architecture mapping
- provider placement
- process and control analysis
- visualization in GuideRail / Viewscape
- traceability into a reference application and, later, into real systems

This document describes the intended structure, model, artifact types, linking rules, authoring approach, and prompting strategy for building `nebulus-landscape` domain by domain.

---

## Core Intent

The repo should answer these questions clearly:

- What are the important business domains in the banking / fintech landscape?
- What enduring capabilities exist within those domains?
- What real user or business journeys occur across those capabilities?
- How are those journeys operationally executed?
- What risks and controls are present?
- What systems, interfaces, and messages participate?
- Where do providers fit?
- How do we link conceptual truth to executable realization in `nebulus-financial`?

The repo should *not* be optimized first for:
- vendor comparison as the primary taxonomy
- arbitrary technical implementation detail
- generic thought-piece writing
- documentation theater without a structured model behind it

The landscape should remain:

> **business truth first, process/control truth second, technical realization third, provider mapping fourth**

That order matters.

---

## Strategic Role of `nebulus-landscape`

`nebulus-landscape` should serve four jobs at once.

## 1. Canonical business architecture model
It defines the durable business shape of the space:
- domains
- capabilities
- value streams
- journeys
- processes
- risks and controls

## 2. Canonical research repository
It preserves the authored knowledge and research behind that model:
- domain writeups
- capability definitions
- provider research
- source references
- open questions
- confidence levels

## 3. Visualization substrate
It provides the structured objects and relationships needed for:
- GuideRail
- Viewscape
- future progression, perspective, and route models
- landscape and sequence-aware views
- provider-in-context exploration

## 4. Bridge to executable realization
It becomes the “source of intent” that `nebulus-financial` can reference using stable IDs for:
- workflows
- APIs
- UI flows
- systems/components
- controls
- tests

That bridge is one of the most important reasons to structure the repo carefully.

---

## Design Principles

## 1. Capability-first, not vendor-first
The landscape should be organized around durable business capabilities, not around provider categories or company names.

Providers enrich the model.
They do not define it.

## 2. Journey and process are not the same
- **Journey** = the real user or business path
- **Process** = the operational execution of that path

They are linked, but they are not interchangeable.

## 3. Value stream frames, but does not dominate
Value streams are important as higher-order business flow framing, but they should not swallow capabilities or journeys.

## 4. Risk/control thinking is integral, not bolted on
Risk and control should not be afterthoughts.
They should be attached directly to processes, journeys, capabilities, and provider/system participation where relevant.

## 5. Provenance matters
Every meaningful artifact should be traceable to:
- a source
- an author
- a confidence level
- an open question if uncertainty exists

## 6. Structured model plus readable docs
The repo should support:
- human-readable narrative docs
- structured YAML/JSON-style model artifacts
- explicit mappings between them

It cannot be only prose and it cannot be only machine objects.

## 7. Future ingestion should not warp the current model
The repo should be designed so future ingestion from:
- codebases
- Figma
- BPMN
- service catalogs
- API registries
- infra/config sources

can enrich the model without replacing the human-authored conceptual truth.

---

## Relationship to `nebulus-financial`

The clean delineation is:

## `nebulus-landscape` owns
- business architecture truth
- capability taxonomy
- value streams
- journey definitions
- process models
- risk/control model
- provider placement
- research/provenance
- cross-domain conceptual relationships

## `nebulus-financial` owns
- executable implementation
- APIs
- workflows
- UI screens
- code structure
- technical architecture realization
- tests
- runtime behavior
- implementation details

### The bridge
`nebulus-financial` should reference canonical IDs from `nebulus-landscape`.

Examples:
- journey IDs
- process IDs
- capability IDs
- control IDs
- system IDs

This gives you:
- traceability
- explainability
- visual linkage
- less hand-wavy architecture talk

The correct relationship is:

> `nebulus-landscape` describes canonical business and control truth.  
> `nebulus-financial` realizes selected parts of that truth in executable form.

---

## Recommended Repository Structure

```text
nebulus-landscape/
  README.md

  docs/
    principles/
      modeling-principles.md
      naming-conventions.md
      provenance-policy.md
      confidence-levels.md
      provider-placement-rules.md
      process-vs-journey.md

    domains/
      customer/
        domain.md
        capabilities/
          customer-onboarding.md
          customer-profile.md
          consent-management.md
        journeys/
          open-retail-account.md
          update-customer-profile.md
        processes/
          retail-account-onboarding.md
        risks-controls/
          onboarding-fraud-controls.md
        providers/
          identity-providers.md
        mappings/
          customer-map.md

      identity-access/
        domain.md
        capabilities/
        journeys/
        processes/
        risks-controls/
        providers/
        mappings/

      accounts/
      payments/
      cards/
      risk-fraud/
      orchestration-control/
      channels-experience/
      networks-schemes/
      data-rewards/
      servicing/
      ledger-core/

    cross-domain/
      value-streams/
        acquire-and-activate-customer.md
        move-money.md
        service-customer-account.md

      journeys/
        open-savings-account.md
        send-ach-transfer.md
        dispute-card-transaction.md

      providers/
        alloy.md
        plaid.md
        stripe.md
        marqeta.md

      capability-comparisons/
        customer-onboarding-providers.md
        identity-verification-providers.md
        fraud-decisioning-providers.md

      reference-taxonomy/
        domains.md
        capabilities.md
        journey-index.md
        process-index.md
        provider-index.md
        control-index.md
        system-index.md

    sources/
      provider-sources.md
      standards-sources.md
      internal-research-sources.md

  models/
    domains/
      domain.customer.yaml
      domain.identity_access.yaml
      domain.accounts.yaml
      domain.payments.yaml

    capabilities/
      cap.customer_onboarding.yaml
      cap.customer_profile.yaml
      cap.authentication.yaml
      cap.account_opening.yaml
      cap.money_movement.yaml

    value-streams/
      vs.acquire_and_activate_customer.yaml
      vs.move_money.yaml
      vs.service_customer_account.yaml

    journeys/
      journey.open_savings_account.yaml
      journey.send_ach_transfer.yaml

    processes/
      proc.account_opening_v1.yaml
      proc.ach_transfer_v1.yaml

    process-stages/
      stage.account_opening.launch.yaml
      stage.account_opening.collect_application.yaml
      stage.account_opening.verify_identity.yaml

    risks/
      risk.synthetic_identity.yaml
      risk.account_takeover.yaml
      risk.duplicate_payment.yaml

    controls/
      ctrl.idv_before_funding.yaml
      ctrl.transfer_limit_check.yaml
      ctrl.sanctions_screen_before_open.yaml

    providers/
      prov.alloy.yaml
      prov.plaid.yaml
      prov.marqeta.yaml

    provider-mappings/
      map.prov.alloy.yaml
      map.prov.plaid.yaml

    provider-research/
      research.prov.alloy.yaml
      research.prov.plaid.yaml

    systems/
      sys.mobile_app.yaml
      sys.web_app.yaml
      sys.identity_service.yaml
      sys.risk_engine.yaml
      sys.account_service.yaml

    interfaces/
      if.identity_service.verify_identity.yaml
      if.risk_engine.evaluate_application.yaml

    messages/
      msg.identity_verification_request.yaml
      msg.application_risk_decision.yaml

    evidence/
      ev.idv_policy_v1.yaml
      ev.transfer_limits_policy_v1.yaml

    sources/
      src.domain_map_v1.yaml
      src.provider.alloy.site.yaml
      src.process_doc_account_opening_v1.yaml

  mappings/
    capability-to-journey.yaml
    journey-to-process.yaml
    process-to-risk-control.yaml
    provider-to-capability.yaml
    system-to-capability.yaml
    interface-to-system.yaml

  templates/
    domain-template.md
    capability-template.md
    value-stream-template.md
    journey-template.md
    process-template.md
    risk-template.md
    control-template.md
    provider-template.md
    system-template.md

  prompts/
    domain-discovery-prompt.md
    capability-decomposition-prompt.md
    journey-authoring-prompt.md
    process-authoring-prompt.md
    risk-control-prompt.md
    provider-mapping-prompt.md
    reconciliation-prompt.md
```

---

## Why This Structure Works

It separates the repo into three useful layers:

## 1. `docs/`
Human-readable authored artifacts.
These are what you and others can actually read, debate, refine, and teach from.

## 2. `models/`
Canonical structured objects.
These are what tooling, visualization, and future import/export workflows can depend on.

## 3. `mappings/`
Explicit relationships.
These prevent the repo from becoming a set of isolated object files.

This is the right balance between:
- readable thought
- structured data
- durable relationships

---

## Canonical Object Model

The core canonical object set should be:

- Domain
- Capability
- ValueStream
- Journey
- Process
- ProcessStage
- Risk
- Control
- Provider
- System
- Interface
- Message
- EvidenceRef
- SourceRef

You do not need every object type equally mature on day one, but these are the right durable building blocks.

---

## Canonical Schema Guidance

Use YAML for structured model artifacts.

It is:
- readable
- authorable
- diff-friendly
- flexible enough for early growth

## Domain Schema

```yaml
id: domain.customer
name: Customer
description: Business area responsible for customer identity, profile, onboarding, and consent.
capability_ids:
  - cap.customer_onboarding
  - cap.customer_profile
related_domain_ids:
  - domain.identity_access
  - domain.accounts
source_refs:
  - src.domain_map_v1
status: draft
```

## Capability Schema

```yaml
id: cap.customer_onboarding
name: Customer Onboarding
domain_id: domain.customer
description: Capability to initiate, collect, validate, and progress a new customer through onboarding.
value_stream_ids:
  - vs.acquire_and_activate_customer
journey_ids:
  - journey.open_savings_account
process_ids:
  - proc.account_opening_v1
risk_ids:
  - risk.synthetic_identity
control_ids:
  - ctrl.idv_before_funding
provider_ids:
  - prov.alloy
system_ids:
  - sys.mobile_app
  - sys.identity_service
source_refs:
  - src.capability_model_v1
status: draft
```

## Value Stream Schema

```yaml
id: vs.acquire_and_activate_customer
name: Acquire and Activate Customer
description: Higher-order value-producing flow for acquiring, verifying, onboarding, opening, and activating a new retail customer relationship.
domain_ids:
  - domain.customer
  - domain.identity_access
  - domain.accounts
capability_ids:
  - cap.customer_onboarding
  - cap.authentication
  - cap.account_opening
journey_ids:
  - journey.open_savings_account
source_refs:
  - src.value_stream_map_v1
status: draft
```

## Journey Schema

```yaml
id: journey.open_savings_account
name: Open Savings Account
description: End-to-end retail customer journey to open and fund a savings account.
actor: retail_customer
domain_ids:
  - domain.customer
  - domain.identity_access
  - domain.accounts
capability_ids:
  - cap.customer_onboarding
  - cap.authentication
  - cap.account_opening
  - cap.initial_funding
value_stream_id: vs.acquire_and_activate_customer
process_id: proc.account_opening_v1
entry_points:
  - mobile_app
  - web_app
outcomes:
  - account_opened
  - account_opening_pending_review
source_refs:
  - src.journey_map_v1
status: draft
```

## Process Schema

```yaml
id: proc.account_opening_v1
name: Retail Account Opening
journey_id: journey.open_savings_account
description: Operational process for onboarding, verifying, approving, opening, and funding a retail account.
stage_ids:
  - stage.account_opening.launch
  - stage.account_opening.collect_application
  - stage.account_opening.verify_identity
  - stage.account_opening.risk_review
  - stage.account_opening.open_account
  - stage.account_opening.initial_funding
risk_ids:
  - risk.synthetic_identity
  - risk.sanctions_match
control_ids:
  - ctrl.idv_before_funding
  - ctrl.sanctions_screen_before_open
system_ids:
  - sys.mobile_app
  - sys.identity_service
  - sys.risk_engine
  - sys.account_service
source_refs:
  - src.process_doc_account_opening_v1
status: draft
```

## ProcessStage Schema

```yaml
id: stage.account_opening.verify_identity
name: Verify Identity
process_id: proc.account_opening_v1
description: Validate customer identity before account approval and initial funding.
order: 3
system_ids:
  - sys.identity_service
risk_ids:
  - risk.synthetic_identity
control_ids:
  - ctrl.idv_before_funding
source_refs:
  - src.process_doc_account_opening_v1
status: draft
```

## Risk Schema

```yaml
id: risk.synthetic_identity
name: Synthetic Identity Fraud
description: Risk that a fabricated or manipulated identity is used to open an account.
process_ids:
  - proc.account_opening_v1
capability_ids:
  - cap.customer_onboarding
control_ids:
  - ctrl.idv_before_funding
severity: high
source_refs:
  - src.risk_model_v1
status: draft
```

## Control Schema

```yaml
id: ctrl.idv_before_funding
name: Identity Verification Before Funding
description: Customer identity must be verified successfully before initial funding is permitted.
risk_ids:
  - risk.synthetic_identity
process_ids:
  - proc.account_opening_v1
control_type: preventive
owner: domain.identity_access
evidence_refs:
  - ev.idv_policy_v1
source_refs:
  - src.control_framework_v1
status: draft
```

## Provider Schema

```yaml
id: prov.alloy
name: Alloy
category: orchestration_and_decisioning
summary: Identity, fraud, and onboarding orchestration provider.
status: active_research
source_refs:
  - src.provider.alloy.site
  - src.provider.alloy.docs
tags:
  - onboarding
  - identity
  - fraud
```

## Provider Mapping Schema

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
  - src.provider.alloy.docs
```

## Provider Research Schema

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
  - may become a central dependency in onboarding flow design
  - integration shape may vary by institution architecture
open_questions:
  - how configurable are downstream decision explanations?
  - what evidence surfaces are available for control/audit mapping?
source_refs:
  - src.provider.alloy.site
  - src.provider.alloy.docs
last_reviewed: 2026-03-24
```

## System Schema

```yaml
id: sys.identity_service
name: Identity Service
description: Technical service responsible for initiating and evaluating identity verification interactions.
capability_ids:
  - cap.identity_verification
process_ids:
  - proc.account_opening_v1
interface_ids:
  - if.identity_service.verify_identity
source_refs:
  - src.system_model_v1
status: draft
```

## Interface Schema

```yaml
id: if.identity_service.verify_identity
name: Verify Identity
system_id: sys.identity_service
description: Interface for identity verification request and response.
message_ids:
  - msg.identity_verification_request
  - msg.identity_verification_result
source_refs:
  - src.api_spec_identity_v1
status: draft
```

## Message Schema

```yaml
id: msg.identity_verification_request
name: Identity Verification Request
interface_id: if.identity_service.verify_identity
description: Message carrying customer identity payload for verification.
source_refs:
  - src.api_spec_identity_v1
status: draft
```

## EvidenceRef Schema

```yaml
id: ev.idv_policy_v1
name: IDV Policy v1
description: Evidence artifact documenting verification policy and enforcement requirements.
source_refs:
  - src.control_framework_v1
status: active
```

## SourceRef Schema

```yaml
id: src.provider.alloy.docs
type: provider_docs
title: Alloy documentation
provider_id: prov.alloy
notes: Primary source for product behavior and integration observations.
status: active
```

---

## Markdown Artifact Templates

Every markdown artifact should begin with consistent frontmatter.

## Common frontmatter example

```yaml
---
id: cap.customer_onboarding
type: capability
name: Customer Onboarding
domain_id: domain.customer
status: draft
source_refs:
  - src.capability_model_v1
last_reviewed: 2026-03-24
---
```

## Domain Doc Template

```md
# [Domain Name]

## Definition
## Business Purpose
## Scope
## Included Capabilities
## Excluded Capabilities
## Upstream Dependencies
## Downstream Dependencies
## Related Journeys
## Typical Risks and Controls
## Typical Systems and Providers
## Open Questions
## Provenance
```

## Capability Doc Template

```md
# [Capability Name]

## Definition
## Business Outcome
## Scope
## Included Activities
## Excluded Activities
## Upstream Dependencies
## Downstream Dependencies
## Related Value Streams
## Related Journeys
## Related Processes
## Risks
## Controls
## Typical Systems
## Typical Providers
## Open Questions
## Provenance
```

## Value Stream Doc Template

```md
# [Value Stream Name]

## Definition
## Business Objective
## Scope
## Included Domains
## Included Capabilities
## Representative Journeys
## Representative Processes
## Risk and Control Themes
## Systems and Provider Themes
## Open Questions
## Provenance
```

## Journey Doc Template

```md
# [Journey Name]

## Objective
## Primary Actor
## Trigger
## Preconditions
## Happy Path
## Alternate Paths
## Failure / Exception Paths
## Related Domains
## Related Capabilities
## Value Stream Context
## Supporting Process
## Risks and Controls
## Typical Systems and Providers
## Open Questions
## Provenance
```

## Process Doc Template

```md
# [Process Name]

## Objective
## Scope
## Trigger
## Entry Conditions
## Stage Overview
## Stage-by-Stage Flow
## Decision Points
## Alternate Outcomes
## Risks
## Controls
## Evidence / Audit Considerations
## Systems Involved
## Provider Participation
## Open Questions
## Provenance
```

## Risk Doc Template

```md
# [Risk Name]

## Definition
## Why It Matters
## Where It Appears
## Severity
## Related Capabilities
## Related Processes
## Related Controls
## Evidence Considerations
## Open Questions
## Provenance
```

## Control Doc Template

```md
# [Control Name]

## Definition
## Risk Addressed
## Control Type
## Trigger / Conditions
## Owner
## Process Placement
## Evidence / Proof
## Failure Implications
## Open Questions
## Provenance
```

## Provider Doc Template

```md
# [Provider Name]

## Summary
## Category
## What It Appears To Do
## What It Does Not Do
## Capabilities Touched
## Journeys Touched
## Processes Touched
## Risks and Controls Affected
## Systems / Interfaces Implied
## Strengths
## Constraints
## Open Questions
## Provenance
```

---

## Provider Research Model

Provider material should be represented in three layers:

## 1. Provider Profile
The canonical provider identity layer.

## 2. Provider Research
The rich evidence and interpretation layer.

## 3. Provider Mapping
The explicit placement layer tying the provider into:
- capabilities
- journeys
- processes
- risks
- controls
- systems

### Provider folder structure

```text
docs/providers/
  alloy/
    provider.md
    research.md
    capabilities.md
    journeys.md
    processes.md
    risks-controls.md
```

### Provider comparison docs

```text
docs/capability-comparisons/
  customer-onboarding-providers.md
  identity-verification-providers.md
  fraud-decisioning-providers.md
```

### Rule

> Providers should be mapped into capabilities, journeys, processes, risks, controls, and systems. They should not become the organizing taxonomy of the landscape.

---

## Provenance and Confidence

Every meaningful artifact should support:
- source references
- open questions
- confidence if interpretation is involved

This is especially important for:
- provider mappings
- inferred process/risk/control relationships
- system/interface/message placement
- future ingestion outputs

### Suggested confidence values
- high
- medium
- low
- candidate
- disputed

### Why this matters
This keeps the model honest and prevents accidental over-certainty.

---

## Future Ingestion Readiness

The canonical model should be designed so future ingestion can enrich it without warping it.

Potential future sources:
- codebases
- explicit workflows or state machines
- BPMN artifacts
- Figma artifacts
- API registries
- service catalogs
- infra/config sources
- runtime traces

### Important rule
Future-ingested hierarchy, provenance, or technical metadata should enrich the model, but should not override the shared business architecture truth of the repo.

This means:
- importer-friendly fields are good
- source categories are good
- provenance on imported entities is essential
- but the repo remains concept-first, not import-first

---

## Best Research Sequence

Do not try to model the whole industry at once.

Use this sequence:

## Phase 1
Define domains and capabilities.

## Phase 2
Define 5–10 flagship journeys.

## Phase 3
Define processes for those journeys.

## Phase 4
Attach risks and controls.

## Phase 5
Map providers and systems.

## Phase 6
Link `nebulus-financial` artifacts to canonical IDs.

This is how you keep the repo useful instead of turning it into generic mush.

---

## Suggested Domain Order

A strong order for bootstrapping:

1. Customer
2. Identity & Access
3. Accounts
4. Payments
5. Risk & Fraud
6. Orchestration & Control
7. Channels & Experience
8. Cards
9. Networks & Schemes
10. Data & Rewards
11. Servicing
12. Ledger / Core Banking

This order gives you a strong flagship journey set early.

---

## Prompting Strategy

The best prompting pattern is:

1. generate domain artifacts
2. decompose into capabilities
3. generate flagship journeys
4. derive processes
5. attach risks/controls
6. map providers and systems
7. reconcile overlaps and gaps

Do not ask for the entire fintech landscape in one shot.

---

# Deep Thinking Prompts

## 1. Domain Discovery Prompt

```text
You are helping create a canonical business architecture knowledge repo called nebulus-landscape.

Your task is to produce a domain artifact for the domain: [DOMAIN NAME].

Goal:
Produce a high-quality domain document and a matching structured domain object that fits a banking/fintech landscape model.

Modeling rules:
- Treat the domain as a durable business area, not a vendor category.
- Separate domain from capability.
- Do not collapse journeys or processes into the domain definition.
- Use banking/fintech language that is concrete and practically useful.
- Surface ambiguity explicitly.
- Do not pretend completeness if tradeoffs exist.

Output 1: Markdown domain artifact with these sections:
1. Definition
2. Business purpose
3. Scope
4. Included capabilities
5. Excluded capabilities
6. Upstream dependencies
7. Downstream dependencies
8. Related journeys
9. Typical risks and controls
10. Typical systems/providers involved
11. Open questions
12. Provenance notes

Output 2: YAML object with:
- id
- name
- description
- capability_ids
- related_domain_ids
- source_refs
- status

Additional requirements:
- Propose durable capability names under the domain
- Avoid vendor-first framing
- Favor enduring concepts over current implementation details
- Where uncertainty exists, add an “Open questions” subsection
```

## 2. Capability Decomposition Prompt

```text
You are expanding the canonical knowledge model for nebulus-landscape.

Your task is to decompose the domain [DOMAIN NAME] into durable capabilities.

Goal:
Produce capability artifacts that are stable, business-meaningful, and useful for linking to journeys, processes, risks, controls, systems, and providers.

Rules:
- A capability is an enduring business ability.
- Do not confuse capabilities with journeys.
- Do not confuse capabilities with individual technical services.
- Keep names concise and durable.
- Separate neighboring capabilities where the boundary matters.
- Surface overlaps and ambiguity explicitly.

For each proposed capability, output:

Markdown sections:
1. Definition
2. Business outcome
3. Scope
4. Included activities
5. Excluded activities
6. Related value streams
7. Related journeys
8. Likely processes
9. Risks
10. Controls
11. Typical systems/providers
12. Open questions
13. Provenance

YAML fields:
- id
- name
- domain_id
- description
- value_stream_ids
- journey_ids
- process_ids
- risk_ids
- control_ids
- provider_ids
- system_ids
- source_refs
- status

Also provide:
- a short list of recommended flagship capabilities for initial modeling priority
- a short list of neighboring capabilities that are often confused with these
```

## 3. Journey Authoring Prompt

```text
You are authoring a canonical journey artifact for nebulus-landscape.

Journey: [JOURNEY NAME]
Primary domain/capability context: [DOMAIN/CAPABILITY]
Value stream context: [VALUE STREAM NAME OR “propose one if needed”]

Goal:
Produce a realistic business/user journey artifact suitable for linking to process, systems, risks, and providers.

Rules:
- Journey is the real business or user path.
- Do not make this a technical sequence.
- Do not make this a BPMN process.
- Focus on actor path, stages, and meaningful alternate outcomes.
- Keep the journey anchored to real banking/fintech behavior.

Output sections:
1. Objective
2. Primary actor
3. Trigger
4. Preconditions
5. Happy path
6. Alternate paths
7. Failure / exception paths
8. Related domains
9. Related capabilities
10. Value stream framing
11. Supporting process candidates
12. Major risks and controls
13. Typical systems/providers involved
14. Open questions
15. Provenance

Also output matching YAML:
- id
- name
- description
- actor
- domain_ids
- capability_ids
- value_stream_id
- process_id
- entry_points
- outcomes
- source_refs
- status
```

## 4. Process Authoring Prompt

```text
You are authoring a canonical process artifact for nebulus-landscape.

Process: [PROCESS NAME]
Linked journey: [JOURNEY NAME]

Goal:
Produce a process artifact that explains how the journey is operationally executed.

Rules:
- Process is execution flow, not user journey.
- Structure into meaningful stages.
- Include decisions and alternate outcomes where relevant.
- Capture risks and controls in the process, not only in a side note.
- Keep process semantics clear even if BPMN is not explicitly used.

Output sections:
1. Objective
2. Scope
3. Trigger
4. Entry conditions
5. Stage overview
6. Detailed stage-by-stage flow
7. Decision points
8. Alternate outcomes
9. Risks
10. Controls
11. Evidence / audit considerations
12. Systems involved
13. Provider participation
14. Open questions
15. Provenance

Also output YAML:
- id
- name
- journey_id
- description
- stage_ids
- risk_ids
- control_ids
- system_ids
- source_refs
- status

Also include:
- proposed stage IDs
- which steps look like control points
- which steps should later map to technical sequences
```

## 5. Risk / Control Prompt

```text
You are authoring risk and control artifacts for nebulus-landscape.

Focus area:
- domain: [DOMAIN]
- capability: [CAPABILITY]
- journey/process: [JOURNEY OR PROCESS]

Goal:
Identify the most meaningful risks and the controls that mitigate them.

Rules:
- Use practical banking/fintech risk language.
- Risks should be concrete and operationally meaningful.
- Controls should be specific enough to be testable or evidenced.
- Distinguish preventive, detective, and corrective controls where useful.
- Do not invent fake precision where uncertainty exists.

Output:
For each risk:
1. Risk definition
2. Why it matters
3. Where it appears in the process/journey
4. Severity
5. Related controls
6. Evidence considerations
7. Open questions
8. Provenance

For each control:
1. Control name
2. Risk addressed
3. Control type
4. Trigger / when it applies
5. Owner
6. Evidence / proof
7. Failure implications
8. Open questions
9. Provenance

Also emit YAML objects for:
- risk
- control
- process/control links
```

## 6. Provider Mapping Prompt

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

## 7. Reconciliation Prompt

```text
You are reconciling canonical artifacts in nebulus-landscape.

Inputs:
- [LIST OF ARTIFACTS OR PASTED CONTENT]

Goal:
Identify overlap, duplicate concepts, weak naming, missing links, and boundary mistakes.

Review for:
1. Domain vs capability confusion
2. Capability vs journey confusion
3. Journey vs process confusion
4. Missing value stream framing
5. Missing risk/control linkage
6. Weak provider placement
7. Inconsistent IDs or naming
8. Missing provenance
9. Places where the model is too implementation-specific
10. Places where the model is too abstract to be useful

Output:
- recommended corrections
- canonical naming fixes
- merge/split suggestions
- missing relationship links
- confidence warnings
```

---

## Stable ID Strategy

The bridge between landscape and executable systems should be IDs, not vibes.

Examples:
- `domain.customer`
- `cap.customer_onboarding`
- `vs.acquire_and_activate_customer`
- `journey.open_savings_account`
- `proc.account_opening_v1`
- `stage.account_opening.verify_identity`
- `risk.synthetic_identity`
- `ctrl.idv_before_funding`
- `prov.alloy`
- `sys.identity_service`
- `if.identity_service.verify_identity`
- `msg.identity_verification_request`

This is what allows:
- traceability
- visualization
- code linkage
- future import/export logic

---

## How `nebulus-financial` Should Link Back

`nebulus-financial` should reference canonical model IDs in:
- workflow definitions
- API metadata
- UI flow metadata
- architecture docs
- tests
- rules/config

Example:
- onboarding UI flow tagged with `journey.open_savings_account`
- onboarding workflow tagged with `proc.account_opening_v1`
- identity verification service tagged with `cap.identity_verification`
- rule config tagged with `ctrl.idv_before_funding`

That linkage is what turns the landscape into a useful system instead of an ornamental one.

---

## Best Working Rhythm

For each domain, work in this order:

1. write the domain artifact
2. derive the capabilities
3. pick 2–3 flagship journeys
4. define 1–2 flagship processes
5. attach top risks and controls
6. map 1–3 important providers
7. reconcile overlaps and weak naming
8. link selected `nebulus-financial` artifacts once available

That is the safest pattern.

---

## Final Summary

`nebulus-landscape` should become:

- a canonical business architecture model
- a structured research repo
- a provider-aware but not vendor-first landscape
- a risk/control-aware process model
- a substrate for visualization and guided exploration
- a traceable bridge into `nebulus-financial`

The most important long-term rule is:

> `nebulus-landscape` should describe canonical business truth and control truth.  
> Providers, systems, interfaces, and future ingestion enrich that truth.  
> `nebulus-financial` should realize selected parts of it in executable form.

That is the right foundation.
