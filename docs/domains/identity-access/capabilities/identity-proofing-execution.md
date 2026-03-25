---
id: cap.identity_proofing_execution
type: capability
name: Identity Proofing Execution
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Identity Proofing Execution

## Definition

Identity Proofing Execution is the enduring business ability to perform the checks, evidence handling, validation, and verification activities required to establish confidence that a party is who they claim to be for digital or assisted access purposes. It is the capability that executes the trust decisioning work needed before the institution is willing to bind access to a claimant. It is distinct from customer-record creation and distinct from runtime authentication.

## Business Outcome

Produce trustworthy proofing outcomes that allow the institution to: establish access identities with appropriate confidence, support customer onboarding and sensitive recovery flows, reduce impersonation risk, provide downstream domains with evidence-backed trust assertions rather than raw claims.

## Scope

### Included

- Collecting proofing evidence and identity attributes needed for verification
- Validating evidence quality and authenticity
- Verifying claimed identity against trusted or reference sources
- Assessing confidence in the identity claim
- Orchestrating proofing methods such as document checks, data checks, biometric comparisons, liveness checks, or assisted review where applicable
- Recording proofing results, method, confidence, and failure reasons
- Performing re-proofing in high-risk circumstances such as account recovery or certain access reinstatement flows
- Managing proofing escalation to manual review when automated confidence is insufficient

### Excluded

- Creation of the business customer record (cap.customer_identity_establishment in domain.customer)
- Coordination of customer due diligence completion (cap.customer_due_diligence_coordination in domain.customer)
- Broader fraud/AML/sanctions decisioning (domain.risk_fraud)
- Login-time authentication using already-bound authenticators (cap.authentication)
- Ongoing session continuity (cap.session_and_device_trust_management)

## Dependencies

### Upstream

- domain.customer supplies claimed identity context
- domain.channels_experience supplies capture surfaces
- External identity evidence sources
- domain.orchestration_control may coordinate escalation

### Downstream

- cap.access_identity_establishment
- cap.identity_recovery_and_reset_management
- cap.authentication and cap.step_up_and_adaptive_authentication consume proofing assurance context indirectly
- domain.customer onboarding consumes proofing results
- domain.accounts may require satisfactory proofing

## Value Streams

- vs.identity_assurance
- vs.customer_onboarding
- vs.trusted_customer_relationship
- vs.account_recovery

## Journeys

- journey.verify_identity_for_onboarding
- journey.recover_account_access
- journey.perform_identity_reproof_for_high_risk_change
- journey.employee_access_request_and_approval
- journey.open_savings_account

## Processes

- proc.identity_proofing_v1
- proc.proofing_evidence_collection
- proc.identity_verification_decision
- proc.manual_proofing_review
- proc.reproof_for_account_recovery

## Risks

- Weak or inconsistent proofing allows impersonation
- Fraudulent or synthetic identity evidence passes verification
- Proofing methods are mismatched to action risk
- Proofing outcomes are not recorded with sufficient evidence
- Automated proofing fails unfairly or generates excessive false negatives
- Recovery re-proofing is weaker than onboarding proofing

## Controls

- ctrl.identity_proofing_policy_by_use_case
- ctrl.identity_evidence_validation_rules
- ctrl.proofing_confidence_thresholds
- ctrl.manual_review_for_ambiguous_proofing
- ctrl.proofing_result_audit_trail
- ctrl.reproofing_controls_for_recovery

## Systems and Providers

### Systems

- Identity proofing orchestration platform
- Document and evidence verification service
- Biometric/liveness verification service
- Proofing result repository
- Assisted proofing review workbench

### Providers

- Socure
- Alloy
- Trulioo
- LexisNexis Risk Solutions
- Mitek

## Open Questions

- Should proofing later split into consumer proofing, workforce proofing, and recovery re-proofing?
- How much biometric comparison should be modeled inside proofing versus as a shared trust primitive?
- Where should the line sit between proofing failure and fraud investigation once domain.risk_fraud is modeled deeply?

## Provenance

Strongly supported by NIST digital identity guidance which treats identity proofing as distinct from authentication. FFIEC guidance reinforces risk-appropriate authentication and access practices at financial institutions.
