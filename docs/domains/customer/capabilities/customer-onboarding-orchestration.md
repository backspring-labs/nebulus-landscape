---
id: cap.customer_onboarding_orchestration
type: capability
name: Customer Onboarding Orchestration
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Onboarding Orchestration

## 1. Definition

Customer Onboarding Orchestration is the enduring business ability to coordinate the customer-owned onboarding journey across milestones, dependencies, handoffs, exceptions, and completion states, regardless of which underlying domains perform individual tasks or decisions.

## 2. Business Outcome

Provide a coherent and governable onboarding progression so the institution can move customers from applicant to ready-for-relationship or ready-for-account-opening status with transparency, reduced leakage, and controlled exception handling.

## 3. Scope

### Included activities
- Defining customer-owned onboarding stages, milestones, and completion criteria
- Tracking onboarding status across intake, identity, due diligence, disclosures, funding prerequisites, and product handoffs
- Coordinating handoffs across customer, identity, risk, operations, and account-opening participants
- Managing pending items, checkpoints, exceptions, and rework loops
- Providing onboarding progress visibility to staff and customers where appropriate
- Managing timeout, abandonment, manual-review, and resume states
- Coordinating multi-party onboarding such as joint applicants or business principals where relevant
- Governing what constitutes onboarding completion versus failure, withdrawal, or suspension
- Maintaining the relationship-level onboarding case/thread from a customer perspective

### Excluded activities
- Ownership of generic orchestration engines, BPM platforms, or enterprise workflow tooling (`domain.orchestration_control`)
- UI step presentation and guided screens (`domain.channels_experience`)
- Decision logic for fraud, AML, sanctions, or identity proofing (`domain.risk_fraud`, `domain.identity_access`)
- Account booking and contract activation (`domain.accounts`)
- Complaint/dispute/case servicing after onboarding (`domain.servicing`)

## 4. Upstream Dependencies
- Application payloads and states from `cap.customer_application_intake`
- Prospect context from `cap.customer_acquisition_management`
- Existing-party signals from `cap.customer_party_resolution`
- Due diligence requirement statuses from `cap.customer_due_diligence_coordination`
- Identity verification results from `domain.identity_access`
- Risk decision statuses from `domain.risk_fraud`
- Workflow tooling and state-transition infrastructure from `domain.orchestration_control`

## 5. Downstream Dependencies
- `cap.customer_identity_establishment` once onboarding reaches authoritative-customer status
- `domain.accounts` for account opening and product fulfillment
- `domain.servicing` when onboarding exceptions become operational cases
- Analytics and performance measurement domains for funnel and SLA reporting
- Customer communications and alerts capabilities where notifications are modeled elsewhere

## 6. Related Value Streams
- `vs.customer_onboarding`
- `vs.deposit_account_opening`
- `vs.operational_control`
- `vs.customer_conversion`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.open_joint_account`
- `journey.branch_assisted_customer_onboarding`
- `journey.resume_abandoned_application`
- `journey.existing_customer_adds_new_relationship`

## 8. Likely Processes
- Onboarding case initiation
- Milestone management
- Pending-item management
- Exception routing
- Manual review coordination
- Resume/restart progression
- Onboarding completion certification
- Failure, withdrawal, and abandonment handling

## 9. Risks
- Milestones are unclear, causing inconsistent completion decisions
- Applicants are stranded in pending or exception states without ownership
- Downstream decisions are received but not reflected consistently in onboarding status
- Operational teams re-create shadow tracking outside the onboarding record
- Joint or multi-party onboarding becomes desynchronized across participants
- In-flight policy changes create unequal treatment between similar applicants
- Onboarding appears complete before all prerequisites are actually satisfied

## 10. Controls
- Standard onboarding state model with explicit entry/exit criteria
- Dependency matrix for required checkpoints before completion
- SLA and aging controls for pending/exception states
- Escalation paths for manual review and stalled onboarding
- Reconciliation between decision outputs and onboarding-state transitions
- Complete/incomplete certification logic before downstream account activation
- Change governance for onboarding-state rules and milestone definitions
- Audit trail for all milestone changes, overrides, and closures

## 11. Typical Systems/Providers

### Typical systems
- Origination journey managers
- Onboarding work queues
- BPM/case progression views
- Banker/ops workbenches
- Status/event monitoring dashboards
- Customer progress-notification components
- Process mining / funnel analytics tools

### Typical providers
- Pega
- Appian
- Camunda
- ServiceNow
- nCino
- Salesforce Financial Services Cloud
- Fiserv / FIS / Jack Henry onboarding modules
- Alloy or similar orchestration overlays when configured for onboarding coordination

## 12. Open Questions
- Should this capability own the relationship-level onboarding case if `domain.servicing` later introduces a more general case capability?
- How far should this capability go in coordinating post-approval but pre-funding actions before ownership flips to `domain.accounts` or `domain.payments`?
- Should customer communications about pending items be modeled here or in a cross-domain communications capability?
- Is "orchestration" the right canonical name, or should it become "Customer Onboarding Management" to avoid confusion with enterprise workflow tooling?

## 13. Provenance

This capability is separated deliberately from workflow technology ownership. In banking practice, institutions need a business-owned view of onboarding progression even when the actual routing technology is shared enterprise infrastructure.
