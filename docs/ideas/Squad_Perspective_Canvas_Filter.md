# IDEA — Squad Perspective Canvas Filter

## Summary

Add a squad-ownership layer to the nebulus-landscape canonical model that guiderail can render as a **perspective filter on the terrain canvas**, allowing team members to see their scope, boundaries, and cross-domain handoff points without restructuring the domain model.

## Problem

Domain boundaries are durable architectural truth. Delivery team boundaries are not — they shift with reorgs, growth, and strategy changes. But every team needs to understand:

- What capabilities do we own or consume?
- Which journeys do we orchestrate end-to-end?
- Where do we hand off to other teams?
- What's in scope for us versus out of scope?

Today this lives in tribal knowledge, stale spreadsheets, or RACI matrices that nobody maintains. The domain model encodes the **architectural truth** but not the **delivery team overlay**.

## Proposed Solution

### 1. Squad-to-domain mapping artifact in landscape

A YAML mapping file in `mappings/squad-to-domain.yaml` that declares:

```yaml
squads:
  - squad_id: squad.onboarding
    name: Onboarding Squad
    description: >
      Cross-domain delivery team responsible for new customer onboarding
      and existing customer account opening experiences.
    owns_journeys:
      - journey.customer_onboarding
      - journey.open_savings_account
      - journey.open_checking_account
      - journey.open_joint_account
    owns_processes:
      - proc.account_opening_v1
      - proc.customer_onboarding_v1
    consumes_capabilities:
      - cap.customer_onboarding_orchestration
      - cap.customer_application_intake
      - cap.customer_identity_establishment
      - cap.customer_party_resolution
      - cap.customer_due_diligence_coordination
      - cap.customer_consent_management
      - cap.customer_disclosure_attestation_management
      - cap.identity_proofing_execution
      - cap.identity_and_onboarding_risk_decisioning
      - cap.account_opening
      - cap.account_application_and_contract_formation
      - cap.account_party_role_management
      - cap.account_terms_and_feature_management
      - cap.account_funding_readiness_management
    consumes_from_domains:
      - domain.customer
      - domain.identity_access
      - domain.accounts
      - domain.risk_fraud
    accountability: journey_orchestration

  - squad_id: squad.customer
    name: Customer Squad
    description: >
      Owns the steady-state customer record, profile, contact, consent,
      preference, lifecycle, and relationship governance capabilities.
    owns_capabilities:
      - cap.customer_profile_management
      - cap.customer_contact_management
      - cap.customer_preference_management
      - cap.customer_consent_management
      - cap.customer_disclosure_attestation_management
      - cap.customer_relationship_management
      - cap.customer_householding_and_association_management
      - cap.customer_lifecycle_state_management
      - cap.customer_change_management
      - cap.customer_data_issue_remediation
      - cap.customer_record_retention_and_archival
      - cap.customer_exit_management
    owns_journeys:
      - journey.update_contact_information
      - journey.exit_customer_relationship
    provides_to:
      - squad.onboarding
      - squad.servicing
    accountability: capability_ownership
```

### 2. Guiderail perspective filter

Guiderail renders the squad mapping as a perspective:

- **Perspective type:** Could use the existing `"architecture"` type or introduce a new `"squad"` type
- **Highlight rules:** Capabilities and journeys the squad owns or consumes are highlighted; everything else is dimmed
- **Visibility rules:** Optionally hide artifacts that are completely outside the squad's scope
- **Domain boundaries remain visible** so the team member can see where their scope crosses domains

### 3. Canvas experience

A team member opens guiderail and selects their squad perspective:

- **Highlighted in full color:** the capabilities, journeys, and process stages their squad owns
- **Visible but dimmed:** neighboring capabilities they depend on or hand off to
- **Domain boundary lines still visible:** showing where architectural ownership transitions
- **Handoff points called out:** process stages where one squad's ownership ends and another's begins

This gives every team member an immediate answer to "what's my scope?" without reading a RACI matrix or asking a manager.

## Why this is better than a RACI

| RACI | Squad Perspective |
|------|-------------------|
| Activity-centric, fragments across domains | Outcome-centric, follows journeys end-to-end |
| Static document, goes stale | Version-controlled YAML, updated with the model |
| Doesn't show boundaries visually | Rendered on the terrain canvas with domain boundaries |
| Requires someone to maintain it | Updates are diffs in a mapping file |
| Doesn't connect to the architecture | Directly references canonical capability and journey IDs |

## Key design principle

**The domain model never changes because of squad structure.** Domains are architectural truth. Squads are delivery teams. The mapping is a thin, changeable overlay that connects the two.

When a reorg happens:
- Update `squad-to-domain.yaml`
- Domain model stays the same
- Guiderail perspectives update automatically
- New team members see their new scope immediately

## Implementation considerations

- The mapping YAML should reference canonical IDs only — no free-text capability names
- A squad can consume capabilities from multiple domains (the onboarding squad is the canonical example)
- A capability can be consumed by multiple squads (consent management is used by both onboarding and customer squads)
- Squad perspectives should be composable — show "my squad" or "my squad + the squads I hand off to"
- The mapping should support accountability types: `journey_orchestration`, `capability_ownership`, `platform_service`, `operational_support`

## Relationship to the triad

- **nebulus-landscape** owns the squad-to-domain mapping YAML (the data)
- **guiderail** renders it as a perspective filter (the visualization)
- **nebulus-financial** could annotate its code with squad ownership tags that reference the same squad IDs (the bridge)
- **SquadOps** could use squad mappings to determine which agent squad should build which capabilities

## Open questions

- Should squad mappings live in `mappings/` or in a dedicated `squads/` directory?
- Should the mapping support temporal versioning (squad structure as of date X)?
- Should guiderail allow ad-hoc perspective creation or only consume pre-defined squad mappings?
- Should the mapping include individual team members or stay at the squad level?
- How should shared capabilities (consumed by multiple squads) be rendered — highlighted for all squads that use them, or only for the owning squad?
