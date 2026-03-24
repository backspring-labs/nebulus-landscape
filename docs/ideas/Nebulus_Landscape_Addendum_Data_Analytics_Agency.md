# ADDENDUM — Extending the `nebulus-landscape` Canonical Model for Data, Analytics, Intelligence, and Agency

## Purpose

This addendum extends the `nebulus-landscape` canonical model to better support modern banking and fintech realities that were only lightly represented in the original model:

- data generation and consumption
- analytics and intelligence flows
- decisioning
- observability and evidence
- policy and delegated authority
- AI-assisted and agentic operating models

The original model is already strong in:
- business domains
- capabilities
- journeys
- processes
- risks and controls
- providers
- systems, interfaces, and messages

What this addendum does is make the model more complete for environments where:
- decisions are increasingly data-driven
- controls depend on observable evidence
- intelligence layers influence journeys and processes
- agentic or AI-assisted operating patterns may emerge
- product and operational feedback loops matter

This is **not** a proposal to replace the capability-first structure.
It is a proposal to strengthen it.

The core rule still stands:

> `nebulus-landscape` should remain canonical business truth and control truth first.  
> Data, intelligence, provider detail, and future agency models should enrich that truth, not displace it.

---

## Why This Addendum Is Needed

The original model was intentionally grounded in:
- business architecture
- journeys
- process execution
- risk/control thinking
- provider placement
- linkage into `nebulus-financial`

That is the right starting point.

But in a modern platform, especially one meant to be forward-looking, the model is missing or underemphasizing several important realities:

### 1. Data is not just an implementation detail
Journeys and processes create, consume, transform, and depend on data.
Controls often require data.
Decisioning is often impossible without it.

### 2. Analytics and intelligence are not just reporting
Analytics can influence:
- fraud detection
- onboarding conversion
- operational tuning
- segmentation
- rewards
- customer support
- exception handling
- route optimization
- experimentation

### 3. Decisioning deserves clearer treatment
Many key flows rely on:
- rules
- scores
- thresholds
- model outputs
- human review
- policy-based delegation

These are more than generic “system behavior.”

### 4. Observability and evidence matter deeply
The model currently captures risk and control, but it should better represent:
- evidence generation
- telemetry
- audit traces
- explainability surfaces
- replayability
- post-event analysis

### 5. AI and agency are becoming relevant operating patterns
Even if agentic behavior is not central on day one, the landscape should be capable of expressing:
- AI-assisted decision support
- delegated task execution
- approval boundaries
- human-in-the-loop moments
- explainability and accountability

This is especially relevant for the long-term Nebulus vision.

---

## Design Principle for the Extension

This addendum should extend the model in a way that preserves the original hierarchy and intent.

### The model should still be:
- business-architecture-first
- capability-centered
- journey-aware
- process-aware
- risk/control-aware
- provider-aware
- linkable to executable implementation

### The extension should add:
- data-aware modeling
- intelligence-aware modeling
- decision-aware modeling
- evidence-aware modeling
- policy-aware modeling
- agency-aware modeling

### What it should *not* do
It should not:
- make AI the new top-level taxonomy
- collapse business capabilities into technical data pipelines
- turn the repo into a data catalog first
- turn the repo into an LLM playground
- let provider or model tooling define the landscape

---

## Recommended Enhancements to the Canonical Object Model

The original canonical object set was:

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

This should now be extended with additional objects.

## New or Strengthened Canonical Object Types

### DataObject
Represents a meaningful business or operational data object that is produced, consumed, or transformed.

Examples:
- customer_application
- identity_verification_result
- payment_instruction
- fraud_score_input
- sanctions_screen_result

### DataProduct
Represents a curated data asset or output designed for reuse, reporting, analytics, or decisioning.

Examples:
- onboarding_funnel_metrics
- transaction_risk_feature_set
- customer_health_score
- fraud_case_summary_dataset

### Decision
Represents a meaningful business or operational decision point.

Examples:
- approve_account_opening
- route_to_manual_review
- allow_transfer
- decline_card_authorization
- escalate_suspected_fraud

### Policy
Represents a governing rule set or authority boundary that shapes decisions, controls, or delegated behavior.

Examples:
- onboarding_risk_policy
- transfer_limit_policy
- sanctions_review_policy
- agent_assist_escalation_policy

### Metric
Represents a measurable operational or business signal.

Examples:
- onboarding_completion_rate
- identity_verification_success_rate
- false_positive_rate
- average_manual_review_time
- transfer_failure_rate

### Signal
Represents an event, telemetry item, or derived operational indicator used for monitoring, decisioning, or evidence.

Examples:
- repeated_failed_login_signal
- high_velocity_transfer_signal
- identity_mismatch_signal
- card_tokenization_failure_signal

### ModelArtifact
Represents a model or scored logic artifact involved in decisioning or intelligence.

Examples:
- fraud_risk_model_v3
- onboarding_propensity_model
- support_case_triage_model

### AgentRole
Represents an AI-assisted or delegated role in the operating model.

Examples:
- agent_assist_triage
- fraud_case_summary_agent
- customer_support_drafting_agent

### DelegationBoundary
Represents the explicit line between:
- human-only authority
- system-rules authority
- AI-assisted recommendation
- agent-executable action with approval
- autonomous permitted action

### ObservabilityArtifact
Represents logs, traces, telemetry surfaces, or other operational visibility artifacts relevant to process truth and control evidence.

Examples:
- onboarding_trace
- transfer_processing_event_stream
- fraud_review_audit_log

---

## Recommended Updated Canonical Object Set

The canonical model should now be thought of as:

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
- DataObject
- DataProduct
- Decision
- Policy
- Metric
- Signal
- ModelArtifact
- AgentRole
- DelegationBoundary
- ObservabilityArtifact
- EvidenceRef
- SourceRef

Not all of these need to be heavily populated on day one.
But the model should be designed to accommodate them cleanly.

---

## How the Existing Model Should Be Enhanced

## 1. Capabilities should gain richer data/intelligence linkages

Capabilities should not only point to:
- journeys
- processes
- risks
- controls
- providers
- systems

They should also be able to point to:
- data objects produced/consumed
- decisions made
- policies applied
- metrics used to measure success
- signals used for operational awareness
- model artifacts used for intelligence or scoring

### Example enhancement to a capability schema

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
data_object_ids:
  - data.customer_application
  - data.identity_verification_result
decision_ids:
  - dec.approve_account_opening
policy_ids:
  - pol.onboarding_risk_policy
metric_ids:
  - metric.onboarding_completion_rate
signal_ids:
  - signal.identity_mismatch
model_artifact_ids:
  - model.fraud_risk_model_v3
source_refs:
  - src.capability_model_v1
status: draft
```

This makes the capability object much more useful as a hub.

---

## 2. Journey artifacts should reflect data, decision, and intelligence touchpoints

Journeys should still remain:
- user/business path artifacts
- not technical flows
- not BPMN processes

But they should be enhanced to include:
- important decisions encountered
- major data exchanged or surfaced
- visible signals or evidence points
- AI-assisted touchpoints if any exist
- customer-facing preview of intelligence-driven behavior

### Journey enhancement sections
Add sections like:
- Key decisions encountered
- Important data objects
- Signals / triggers visible in the journey
- AI-assisted or system-assist moments
- Metrics of journey health / success

This helps keep the model honest about what the customer or operator experiences.

---

## 3. Process artifacts should become the primary home for decisions, controls, signals, and evidence

Process remains the strongest bridge object in the model.

It should now also become the primary home for:
- decision objects
- signals
- policies
- evidence generation points
- observability hooks
- delegation boundaries

### Why
Process is where:
- execution happens
- controls are enforced
- evidence is created
- monitoring becomes meaningful
- human/system/AI boundary decisions often occur

### Process enhancement sections
Add to process docs:
- Data produced and consumed by stage
- Decisions by stage
- Signals and thresholds
- Policy application points
- Evidence and observability surfaces
- Delegation / approval boundaries
- AI-assisted touchpoints, if present

---

## 4. Risk and control artifacts should become more evidence- and intelligence-aware

Risk and control docs should not only say:
- what risk exists
- what control mitigates it

They should also say:
- what signals indicate the risk
- what data supports detection
- what evidence proves the control occurred
- whether the control is rules-based, model-assisted, or human-reviewed
- whether AI/agents participate and what the approval boundary is

### Example control enhancement

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
policy_ids:
  - pol.onboarding_risk_policy
signal_ids:
  - signal.identity_mismatch
data_object_ids:
  - data.identity_verification_result
evidence_refs:
  - ev.idv_policy_v1
observability_artifact_ids:
  - obs.onboarding_trace
source_refs:
  - src.control_framework_v1
status: draft
```

This makes controls much stronger for future operating-model realism.

---

## 5. Providers should be mapped not just to capabilities, but also to data, decisioning, and evidence responsibilities

The provider model should be extended so it can answer:

- what data does this provider produce or consume?
- what decisions does it influence?
- what policies does it participate in?
- what signals does it emit?
- what evidence surfaces does it support?
- what model artifacts or decisioning logic does it bring?

### Example enhancement to provider mapping

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
data_object_ids:
  - data.identity_verification_result
decision_ids:
  - dec.route_to_manual_review
signal_ids:
  - signal.identity_mismatch
roles:
  - orchestration
  - decisioning
  - verification
confidence: medium
source_refs:
  - src.provider.alloy.docs
```

This is much more reflective of real provider participation.

---

## Recommended New Domain Families or Cross-Cutting Layers

There are two good ways to represent these enhancements:
- as full domains where the business meaning justifies it
- as cross-cutting layers where they influence many domains

My recommendation is a mix.

## Candidate domain families

### Data & Analytics
Covers:
- data capture
- data products
- reporting
- performance measurement
- experimentation inputs
- analytical reuse

### Decisioning & Intelligence
Covers:
- rules
- models
- scores
- decision services
- explainability
- optimization logic

### Orchestration, Policy & Delegation
Extends the earlier orchestration/control idea to include:
- workflow/routing
- policy application
- delegated authority
- approval boundaries
- human-in-the-loop constraints
- agentic action boundaries

## Strong cross-cutting concerns
Even if not all become standalone domains, these should be treated as first-class cross-cutting layers:
- observability
- audit/evidence
- AI-assisted actions
- delegation boundaries
- model-driven decision support

---

## Recommended Folder Structure Enhancements

Extend the repo like this:

```text
nebulus-landscape/
  docs/
    domains/
      data-analytics/
        domain.md
        capabilities/
        journeys/
        processes/
        risks-controls/
        providers/
        mappings/

      decisioning-intelligence/
        domain.md
        capabilities/
        journeys/
        processes/
        risks-controls/
        providers/
        mappings/

      orchestration-policy-delegation/
        domain.md
        capabilities/
        journeys/
        processes/
        risks-controls/
        providers/
        mappings/

    cross-domain/
      metrics/
        onboarding_metrics.md
        transfer_health_metrics.md
      policies/
        onboarding_risk_policy.md
        transfer_limit_policy.md
      agency/
        agent-assist-patterns.md
        delegation-boundaries.md
      observability/
        evidence-model.md
        process-observability.md

  models/
    data-objects/
      data.customer_application.yaml
      data.identity_verification_result.yaml

    data-products/
      dprod.onboarding_funnel_metrics.yaml
      dprod.transaction_risk_feature_set.yaml

    decisions/
      dec.approve_account_opening.yaml
      dec.route_to_manual_review.yaml

    policies/
      pol.onboarding_risk_policy.yaml
      pol.transfer_limit_policy.yaml

    metrics/
      metric.onboarding_completion_rate.yaml
      metric.false_positive_rate.yaml

    signals/
      signal.identity_mismatch.yaml
      signal.high_velocity_transfer.yaml

    model-artifacts/
      model.fraud_risk_model_v3.yaml
      model.support_case_triage_model.yaml

    agent-roles/
      agent.fraud_case_summary.yaml
      agent.customer_support_drafting.yaml

    delegation-boundaries/
      delb.manual_review_required_for_high_risk.yaml

    observability/
      obs.onboarding_trace.yaml
      obs.transfer_event_stream.yaml
```

This preserves the existing model but makes it more complete.

---

## Recommended New Artifact Templates

## Data Object Template

```md
# [Data Object Name]

## Definition
## Purpose
## Producing Systems / Processes
## Consuming Systems / Processes
## Data Sensitivity / Classification
## Related Decisions
## Related Controls
## Related Metrics / Signals
## Open Questions
## Provenance
```

## Decision Template

```md
# [Decision Name]

## Definition
## Why the Decision Exists
## Inputs
## Policies Applied
## Signals Considered
## Model / Rule Logic
## Possible Outcomes
## Human Review / Approval Boundary
## Evidence / Explainability
## Open Questions
## Provenance
```

## Policy Template

```md
# [Policy Name]

## Definition
## Scope
## Decisions Governed
## Risks Addressed
## Controls Enforced
## Escalation / Delegation Rules
## Evidence Requirements
## Open Questions
## Provenance
```

## Metric Template

```md
# [Metric Name]

## Definition
## Business or Operational Purpose
## Calculation Logic
## Data Sources
## Related Capabilities / Journeys / Processes
## Decision or Alert Usage
## Open Questions
## Provenance
```

## Agent Role Template

```md
# [Agent Role Name]

## Purpose
## Scope of Action
## Inputs
## Allowed Outputs
## Delegation Boundary
## Human Approval Requirements
## Risks
## Controls
## Evidence / Logging Requirements
## Open Questions
## Provenance
```

---

## Recommended Prompt Extensions

Use these prompts to enrich the model domain by domain.

## Data / Analytics Prompt

```text
You are extending the canonical repo nebulus-landscape.

Focus area:
- domain: [DOMAIN]
- capability: [CAPABILITY]
- journey/process: [JOURNEY OR PROCESS]

Goal:
Identify the important data objects, data products, metrics, and signals relevant to this part of the landscape.

Produce:
1. important data objects produced and consumed
2. data products derived from those flows
3. key operational and business metrics
4. signals relevant to monitoring, decisioning, or control
5. where these elements attach to journey, process, risks, and controls
6. open questions
7. provenance notes

Rules:
- capability-first, not data-warehouse-first
- keep the objects business-relevant
- distinguish between raw data object, derived metric, and decisioning signal
- do not invent fake precision
```

## Decisioning / Intelligence Prompt

```text
You are extending the canonical repo nebulus-landscape.

Focus area:
- domain: [DOMAIN]
- capability: [CAPABILITY]
- process: [PROCESS]

Goal:
Identify the meaningful decisions, rules, model artifacts, and intelligence layers that influence this process.

Produce:
1. key decisions
2. decision inputs
3. policies governing those decisions
4. model-assisted or rule-assisted logic
5. explainability/evidence implications
6. risks and controls impacted
7. open questions
8. provenance notes

Rules:
- distinguish rules, decisions, models, and policies
- be explicit where human review is required
- separate recommendation from authority
```

## AI / Agency Prompt

```text
You are extending the canonical repo nebulus-landscape.

Focus area:
- domain/process/context: [CONTEXT]

Goal:
Identify where AI-assisted or agentic behavior could exist in this landscape, and model it safely and explicitly.

Produce:
1. candidate agent or assist roles
2. what inputs they use
3. what outputs they may produce
4. delegation boundaries
5. approval and escalation requirements
6. explainability and evidence requirements
7. risks introduced
8. controls required
9. open questions
10. provenance notes

Rules:
- do not anthropomorphize
- separate recommendation from authority
- define what remains human-only
- define what must remain auditable
```

---

## Enhancement to Provider Research Model

Provider research should now also include:
- data objects produced/consumed
- decisions influenced
- policy touchpoints
- signal generation
- model/artifact involvement
- evidence/observability surfaces
- AI/assistive participation if any exists

This makes provider mapping far more useful for modern platforms.

---

## Enhancement to the Bridge Into `nebulus-financial`

The bridge into `nebulus-financial` should eventually include references not only to:
- capability
- journey
- process
- control

but also to:
- decision IDs
- policy IDs
- metric IDs
- signal IDs
- data object IDs
- model artifact IDs
- agent role IDs
- delegation boundary IDs

This will make the reference application more explainable and more mappable back into the landscape.

---

## Updated Summary of What `nebulus-landscape` Should Become

`nebulus-landscape` should become:

- a canonical business architecture model
- a structured research repo
- a provider-aware but not vendor-first landscape
- a risk/control-aware process model
- a data, analytics, and intelligence-aware model
- an observability and evidence-aware model
- a substrate for visualization and guided exploration
- a traceable bridge into `nebulus-financial`

It should also make room for:
- decisioning
- policy and delegation
- AI-assisted and agentic operating models
- explainability and accountability
- operational learning loops

### The most important long-term rule

> `nebulus-landscape` should describe canonical business truth and control truth.  
> Providers, systems, interfaces, data, intelligence, observability, and future agency models should enrich that truth.  
> `nebulus-financial` should realize selected parts of it in executable form.

---

## Final View

This addendum does not change the original direction.
It completes it.

Without this extension, the model risks underrepresenting:
- modern data-driven decisioning
- evidence and observability
- agentic operating patterns
- the true role of intelligence in journeys and processes

With this extension, the repo becomes a much more powerful substrate for:
- research
- architecture
- product thinking
- controls
- visualization
- forward-looking banking platform design

That is the right enhancement path.
