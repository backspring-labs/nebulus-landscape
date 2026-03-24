---
id: cap.customer_relationship_management
type: capability
name: Customer Relationship Management
domain_id: domain.customer
status: draft
source_refs:
  - src.cfpb_reg_b_12cfr1002
  - src.cfpb_udaap_examination_procedures
  - src.ecfr_glba_safeguards_16cfr314
last_reviewed: 2026-03-24
confidence: low
---

# Customer Relationship Management

## 1. Definition

Customer Relationship Management is the business ability to maintain the institution's relationship context for a customer: ownership and assignment, relationship roles and hierarchies, service tier/segment, and related relationship metadata that governs how the institution organizes and manages the relationship.

This is not a "CRM tool" definition. It is the durable business capability for relationship context and its governance.

## 2. Business Outcome
- Clear ownership and accountability for customer relationships (who services, who approves, who manages).
- Consistent relationship segmentation and service-tier treatment across channels and business lines.
- Correct relationship-role representation (e.g., household context, joint relationships, business relationships as customer contexts).
- Better customer experience through coherent routing and servicing posture.

## 3. Scope

### Included activities
- Maintain relationship context attributes: segment and tier assignments (retail mass, affluent, private, SMB tiers, etc.); relationship owner assignment (relationship manager, servicing team assignment, branch of record); relationship roles (e.g., primary/secondary relationship view, household/linked relationships where applicable).
- Maintain relationship hierarchies and groupings where modeled: household/group constructs (if the institution uses them at the customer relationship level); business relationship context (relationship team, service model).
- Maintain relationship profitability/service-level metadata if used as operational context: service tier flags, eligibility flags, internal treatment levels.
- Provide governed relationship context to downstream routing, servicing, and analytics.

### Excluded activities
- Account ownership and legal contractual roles inside the account contract (domain.accounts).
- Credit decisioning, underwriting, pricing, or adverse action (domain.risk_fraud / lending domains), though relationship context may be an input.
- Servicing case management and complaint handling (domain.servicing).
- Analytical segmentation products and models (domain.data_rewards owns analytics; Customer owns the operational segment/tier "record of assignment").
- Authentication/authorization for changing relationship assignments (domain.identity_access).

## 4. Upstream Dependencies
- cap.customer_profile_management (relationship context depends on correct customer identity and type).
- cap.customer_party_resolution (relationship context must attach to the correct customer).
- domain.data_rewards (may recommend segments/tier or propensity; Customer records the operational assignment).
- domain.servicing (service tier may be derived from servicing performance or case history).
- domain.orchestration_control (policy workflows for assignment changes and approvals).

## 5. Downstream Dependencies
- domain.servicing (routing, prioritization, service posture, agent context).
- domain.channels_experience (personalization and routing).
- domain.accounts (relationship context can inform product packaging, but must not override account contract).
- domain.risk_fraud (relationship context can influence monitoring intensity; decision ownership remains there).
- cap.customer_preference_management (relationship segment can determine available preference options in some institutions).

## 6. Related Value Streams
- `vs.relationship_management_and_service`
- `vs.customer_record_maintenance`
- `vs.customer_engagement_and_growth`

## 7. Related Journeys
- `journey.assign_relationship_owner`
- `journey.update_service_tier`
- `journey.manage_household_relationships`
- `journey.add_joint_relationship_context`
- `journey.open_savings_account`

## 8. Likely Processes
- proc.relationship_owner_assignment
- proc.segment_and_tier_assignment
- proc.householding_and_linking
- proc.relationship_role_management
- proc.relationship_context_audit

## 9. Risks
- risk.incorrect_relationship_assignment (wrong owner; customer mishandled; SLA breaches).
- risk.role_or_household_linking_privacy_breach (inappropriate linking exposes data to wrong parties).
- risk.unfair_or_discriminatory_treatment (segment/tier drives differential treatment in ways that create legal/regulatory risk).
- risk.misleading_service_tier_presentation (UDAAP-type risk when customers are misled about benefits or eligibility).
- risk.unauthorized_assignment_changes (insiders or compromised agents alter tiers/ownership).

## 10. Controls
- ctrl.relationship_assignment_approval_workflow (maker-checker for sensitive assignments).
- ctrl.householding_rules_and_consent_controls (policy gating for linking; record of authorization where required).
- ctrl.segment_governance_and_periodic_review (clear criteria, auditability, change control).
- ctrl.auditable_relationship_context_changes (tamper-evident logs, reason codes).
- ctrl.fairness_and_udaap_review_for_segmentation_use (review when segmentation/tiering impacts protected outcomes or customer treatment).
- ctrl.safeguards_program_alignment (customer relationship context protected as customer information).

## 11. Typical Systems/Providers

### Typical systems
- CRM / relationship management platform
- Customer master + relationship hierarchy store
- Router/prioritization engines (consumers, not owners)
- Analytics/segmentation recommendation engines (inputs from data_rewards)

### Representative providers
- Salesforce Financial Services Cloud
- Pega CRM/service applications
- FIS/Fiserv relationship modules (varies by core)
- Custom data products for householding/relationship graph

## 12. Open Questions
- Should this capability be split into smaller, more durable capabilities: relationship ownership & assignment, segment/service tier management, relationship/household hierarchy management?
- What minimum governance is required to prevent segmentation from creating disparate treatment or misleading treatment?
- What is the correct repository boundary for "profitability" attributes (Customer record vs data_rewards analytics)?

## 13. Provenance

Derived from common banking relationship operating models and segmentation/service tier practices. Included explicit regulatory-risk framing because relationship context often influences customer treatment. Confidence is low because institutions vary widely: some treat relationship context as purely analytical, others as operational truth that affects servicing and offers.
