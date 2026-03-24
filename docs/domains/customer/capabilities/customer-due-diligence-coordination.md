---
id: cap.customer_due_diligence_coordination
type: capability
name: Customer Due Diligence Coordination
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Due Diligence Coordination

## 1. Definition

Customer Due Diligence Coordination is the enduring business ability to identify, collect, track, and progress customer due diligence requirements and evidence for onboarding and relationship establishment while external risk, AML, sanctions, and identity decisions remain owned by neighboring domains.

## 2. Business Outcome

Ensure that the institution gathers the right due diligence information, artifacts, attestations, and status evidence at the right time so that required onboarding decisions can be made and evidenced without making the Customer domain itself the owner of those decisions.

## 3. Scope

### Included activities
- Determining what customer due diligence information and documents are required for a given onboarding context
- Coordinating collection of CIP/CDD-related data and evidence from the applicant
- Tracking pending, received, waived, expired, and satisfied due diligence requirements
- Managing escalation from standard due diligence to enhanced due diligence collection requirements when triggered by external policy or decision signals
- Coordinating beneficial ownership / control-person information collection for legal entities where relevant
- Managing relationship-level evidence packaging for downstream review by risk/compliance functions
- Tracking periodicity or freshness requirements when onboarding spans long durations
- Governing due diligence completion status from the customer/onboarding perspective

### Excluded activities
- Final KYC/AML/sanctions/fraud decisions (`domain.risk_fraud`)
- Customer identification and verification execution under CIP/identity proofing (`domain.identity_access`)
- OFAC screening execution, adverse media screening, transaction monitoring, and SAR decisions (`domain.risk_fraud`)
- Generic workflow tooling (`domain.orchestration_control`)
- Ongoing post-onboarding AML monitoring outside customer-relationship setup (`domain.risk_fraud`)
- Credit underwriting or deposit product approval decisions (`domain.risk_fraud` / product domains)

## 4. Upstream Dependencies
- Application data from `cap.customer_application_intake`
- Onboarding stage context from `cap.customer_onboarding_orchestration`
- Product, customer-segment, jurisdiction, and policy requirements from compliance/risk functions
- Verification and proofing outputs from `domain.identity_access`
- Screening and risk triggers from `domain.risk_fraud`

## 5. Downstream Dependencies
- `domain.risk_fraud` for KYC/AML/sanctions/fraud decisioning
- `domain.identity_access` for identity proofing or documentary/non-documentary verification execution
- `cap.customer_identity_establishment` when due diligence sufficiency is a precondition to canonical establishment
- `domain.accounts` for account-opening continuation after due diligence completion
- `domain.servicing` when unresolved due diligence becomes a manual case

## 6. Related Value Streams
- `vs.customer_onboarding`
- `vs.compliance_enablement`
- `vs.bsa_aml_readiness`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_business_deposit_account`
- `journey.complete_pending_kyc_requirements`
- `journey.escalated_due_diligence_review`

## 8. Likely Processes
- Due diligence requirement determination
- CIP/CDD evidence collection
- Beneficial ownership collection
- EDD trigger coordination
- Pending-document chase and follow-up
- Due diligence completion certification
- Requirement refresh or expiration handling

## 9. Risks
- Required due diligence information is not collected before onboarding progresses
- Evidence is collected but not linked clearly to the requirement it satisfies
- Standard due diligence is applied where enhanced due diligence is required
- Expired or stale documentation is used
- Beneficial ownership/control-person collection is incomplete for legal entities
- Staff assume this capability owns the AML decision instead of the evidence coordination
- Requirements differ by channel or product without clear policy rationale

## 10. Controls
- Requirement matrix by customer type, product, jurisdiction, and risk trigger
- Requirement-to-evidence mapping controls
- Due diligence completion gates before downstream progression
- Expiration and freshness validation for documents and attestations
- Escalation controls for EDD-triggered collection
- Beneficial ownership completeness checks where applicable
- Clear RACI separating evidence coordination from AML/fraud decision ownership
- Audit trail for requirement changes, waivers, and completion status

## 11. Typical Systems/Providers

### Typical systems
- KYC intake workbenches
- Compliance document collection portals
- Due diligence requirement engines
- Beneficial ownership intake modules
- Case/queue interfaces for missing evidence
- Evidence repositories and retrieval services

### Typical providers
- Fenergo
- Alloy
- NICE Actimize
- LexisNexis Risk Solutions
- Moody's / Bureau van Dijk
- Refinitiv / LSEG
- Dow Jones Risk & Compliance
- In-house onboarding compliance orchestration layers

## 12. Open Questions
- Should this capability remain broad across retail and small business, or will business-banking/customer-type specialization become necessary?
- How should CIP collection be split from identity proofing execution if the model later adds finer-grained identity capabilities?
- Should beneficial ownership collection eventually be modeled as its own capability if legal-entity onboarding becomes more central?
- Where should waiver authority for missing or alternate evidence be anchored if `domain.orchestration_control` later models policy exceptions generically?

## 13. Provenance

This capability is heavily informed by BSA/AML and CIP/CDD operating patterns. It is intentionally framed as coordination and evidence progression because the customer domain must often gather and track due diligence inputs without owning the formal compliance or identity decisions themselves.
