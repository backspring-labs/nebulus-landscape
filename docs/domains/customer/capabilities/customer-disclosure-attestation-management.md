---
id: cap.customer_disclosure_attestation_management
type: capability
name: Customer Disclosure & Attestation Management
domain_id: domain.customer
status: draft
source_refs:
  - src.uscode_esign_15usc7001
  - src.fdic_consumer_compliance_esign
  - src.ncua_esign_guide
  - src.ecfr_glba_safeguards_16cfr314
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Disclosure & Attestation Management

## 1. Definition

Customer Disclosure & Attestation Management is the business ability to deliver required customer-facing disclosures and agreements (and subsequent updates), and to capture and retain evidentiary records that the customer received, acknowledged, accepted, or certified information.

This capability is distinct from Consent Management:
- Consent Management governs permissions and opt-outs.
- Disclosure/Attestation Management governs documentary evidence of delivery/acknowledgement/acceptance and customer certifications.

## 2. Business Outcome
- Provable compliance with disclosure and agreement delivery obligations.
- Reduced disputes and legal exposure through strong evidencing and version control.
- Faster onboarding and servicing changes by reusing standardized disclosure + acceptance patterns.
- Better auditability: what the customer saw, when, and what version they accepted.

## 3. Scope

### Included activities
- Manage document/disclosure inventory relevant to customer establishment and maintenance: terms and conditions, agreements, privacy notices where acknowledgement is captured, fee schedules, policy updates; customer certifications/attestations (e.g., tax certification, self-certification for certain classifications, acknowledgements of key facts).
- Manage disclosure versions and "what was presented" evidence: document versioning, effective dates, channel of delivery, customer presentment evidence.
- Capture acceptance/acknowledgement events: click-to-accept, signature events, checkbox acknowledgements with audit trail.
- Support electronic records delivery compliance patterns: where required, store evidence of affirmative consent to electronic delivery and withdrawal handling.
- Maintain retention and retrieval for audit, disputes, and servicing.

### Excluded activities
- Executing e-signature cryptography platforms as technical services (implementation detail; owned by underlying systems).
- Authentication/session trust to confirm the right user is accepting the document (domain.identity_access).
- Product-specific document generation rules (often owned by product/account domains; Customer governs the evidence and customer-level agreement acceptance record).
- Dispute/case handling (domain.servicing).

## 4. Upstream Dependencies
- cap.customer_application_intake (collects answers, agreements, and required acknowledgements).
- cap.customer_onboarding_orchestration (milestones: "disclosures accepted," "agreements signed," exception handling).
- cap.customer_profile_management (ties acceptance to customer identity; record retention metadata).
- cap.customer_consent_management (e-delivery consent and certain privacy permissions may be linked).
- domain.channels_experience (presentment UX and customer interaction).
- domain.identity_access (authentication/step-up for sensitive agreements).

## 5. Downstream Dependencies
- domain.accounts (account opening may depend on agreement acceptance evidence).
- domain.servicing (supporting documentation for disputes, complaints, changes).
- domain.risk_fraud (evidence for investigations; e.g., customer certifications; but decisioning remains there).
- cap.customer_due_diligence_coordination (some attestations/certifications can be inputs to due diligence evidence tracking).
- domain.data_rewards (may use "paperless enrolled" status; must respect consent/attestation constraints).

## 6. Related Value Streams
- `vs.disclosures_and_agreements`
- `vs.customer_record_establishment`
- `vs.customer_record_maintenance`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.enroll_in_electronic_statements`
- `journey.accept_updated_terms`
- `journey.update_privacy_notice_acknowledgement`
- `journey.submit_customer_certification`

## 8. Likely Processes
- proc.disclosure_inventory_management
- proc.disclosure_versioning_and_effective_dates
- proc.disclosure_presentment_and_delivery
- proc.capture_acknowledgement_or_acceptance
- proc.non_repudiation_record_management
- proc.document_retention_and_retrieval

## 9. Risks
- risk.disclosure_not_delivered_or_not_provable (cannot evidence delivery/acceptance).
- risk.invalid_esign_consent_flow (e-delivery implemented without proper affirmative consent).
- risk.attestation_repudiation (customer disputes acceptance; inadequate non-repudiation).
- risk.version_mismatch (customer accepted wrong version or version cannot be reconstructed).
- risk.document_data_exposure (sensitive documents accessible to wrong parties).

## 10. Controls
- ctrl.disclosure_version_control (immutable versions; effective dates; mapped to product/relationship context).
- ctrl.esign_affirmative_consent_controls (explicit consent capture + withdrawal handling, where required).
- ctrl.non_repudiation_and_audit_controls (timestamps, signer identity reference, channel, IP/device where appropriate).
- ctrl.secure_document_storage_and_access (encryption, access control, segregation).
- ctrl.retention_schedule_and_legal_hold (align with recordkeeping obligations and litigation holds).
- ctrl.delivery_failure_detection_and_remediation (bounce/return signals routed for follow-up).

## 11. Typical Systems/Providers

### Typical systems
- Document management system (DMS) / content management
- E-signature platform integration
- Disclosure presentment service (UI + backend)
- Audit/event log and evidence store

### Representative providers
- E-signature: DocuSign, Adobe Acrobat Sign, OneSpan Sign
- Content/DMS: OpenText, SharePoint-based solutions
- Archive/evidence: WORM-capable archives (varies by institution)

## 12. Open Questions
- For paperless/e-delivery: should the "affirmative consent" record be modeled as consent (cap.customer_consent_management), attestation, or a linked pair?
- How should the model represent "deemed delivered" vs "proven received" for certain disclosures and channels?
- Which disclosure acknowledgements belong at the customer level vs at the product/account level (domain.accounts), and how should cross-linking work?

## 13. Provenance

Anchored in electronic records and consumer disclosure consent requirements and in the practical need for versioned, retrievable evidence of what customers accepted. Confidence is medium because document obligations vary by product and jurisdiction, but the underlying evidencing pattern is durable.
