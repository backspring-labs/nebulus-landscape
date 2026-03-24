---
id: cap.customer_application_intake
type: capability
name: Customer Application Intake
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Application Intake

## 1. Definition

Customer Application Intake is the enduring business ability to receive, normalize, validate, and manage customer-supplied application and onboarding information so it can be used reliably by onboarding, due diligence, identity, and account-opening processes.

## 2. Business Outcome

Ensure that applicant-provided data enters the institution in a controlled, reusable, and policy-conformant form that reduces rework, improves straight-through onboarding, and supports downstream review, decisioning, and fulfillment.

## 3. Scope

### Included activities
- Receiving application data from digital, branch, call-center, and partner-assisted channels
- Structuring and normalizing applicant data into canonical intake objects
- Managing required-field, format, completeness, and cross-field validation
- Capturing attestations, acknowledgements, and application declarations at intake
- Managing application draft, save-and-resume, submission, withdrawal, and abandonment states
- Collecting supporting documents, images, uploads, and applicant-provided evidence
- Managing intake variants for retail, joint, minor, sole proprietor, and small-business relationship starts where applicable
- Tracking application versions, corrections, and resubmissions
- Capturing initial funding intent or product selection when part of application intake
- Managing intake timestamps, channel source, and applicant interaction provenance

### Excluded activities
- UI rendering and channel-session behavior (`domain.channels_experience`)
- Identity verification execution, document authenticity verification, or biometric proofing (`domain.identity_access`)
- KYC/AML/fraud/sanctions decisions (`domain.risk_fraud`)
- Generic workflow routing engine ownership (`domain.orchestration_control`)
- Canonical customer identity mastering (`cap.customer_identity_establishment`)
- Existing-customer matching logic ownership (`cap.customer_party_resolution`), though intake provides data to it
- Account contract creation, product booking, and account maintenance (`domain.accounts`)

## 4. Upstream Dependencies
- Acquisition context from `cap.customer_acquisition_management`
- Channel capture from `domain.channels_experience`
- Product/offer and disclosure requirements from product, compliance, and legal sources
- Required-data definitions from risk, identity, tax, and account-opening stakeholders

## 5. Downstream Dependencies
- `cap.customer_onboarding_orchestration` to coordinate progression and exceptions
- `cap.customer_party_resolution` for match and dedupe checks
- `cap.customer_due_diligence_coordination` for CDD/EDD collection progression
- `cap.customer_identity_establishment` once enough information exists to form a canonical party
- `domain.identity_access` for proofing and verification flows
- `domain.risk_fraud` for screening and risk decisions
- `domain.accounts` for account-opening continuation after approved onboarding

## 6. Related Value Streams
- `vs.customer_onboarding`
- `vs.deposit_account_opening`
- `vs.compliance_enablement`
- `vs.customer_data_capture`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_joint_account`
- `journey.resume_abandoned_application`
- `journey.branch_assisted_customer_onboarding`
- `journey.small_business_relationship_start`

## 8. Likely Processes
- Application start
- Save and resume
- Applicant data validation
- Document upload and intake
- Attestation capture
- Application submission
- Correction and resubmission
- Abandonment and withdrawal handling

## 9. Risks
- Material applicant data is missing, malformed, or inconsistent
- Required declarations are not captured or cannot be evidenced later
- Intake accepts stale or conflicting data across channels
- Uploaded documents are linked to the wrong application or applicant
- Joint-applicant or beneficial-owner structures are captured incompletely
- Excessive friction causes abandonment and channel fallout
- Personally identifiable information is exposed through weak draft-handling or attachment controls
- Rules differ by channel, creating unfair or inconsistent onboarding outcomes

## 10. Controls
- Canonical input data standards and field definitions
- Required-field and cross-field validation rules
- Channel-consistent application schemas
- Versioned attestation and disclosure capture
- Attachment indexing and application-to-document binding controls
- Save-and-resume session safeguards and draft expiration policies
- Submission completeness gates before downstream progression
- Maker/checker or exception review for manual corrections in assisted channels
- Audit trail for applicant changes, timestamps, and submission events

## 11. Typical Systems/Providers

### Typical systems
- Account/application origination platforms
- Digital onboarding platforms
- Document upload and image repositories
- e-sign / attestation capture platforms
- Branch and call-center intake workbenches
- Application rules engines
- Content services for disclosures and forms

### Typical providers
- Alkami
- Q2
- MeridianLink
- nCino
- Temenos
- Finastra
- Fiserv
- FIS
- Jack Henry
- DocuSign
- Adobe Acrobat Sign

## 12. Open Questions
- Should attestation/disclosure capture remain part of intake or be split into a distinct capability if the model later emphasizes disclosure governance?
- How far should intake extend for SMB and legal-entity starts before business-banking onboarding deserves a separate specialization?
- Should funding intent, promo codes, and product option selection remain within intake or move to journey-specific layers?
- Where should partial applications that never become canonical applicants be retained in the model?

## 13. Provenance

This capability is strongly grounded in practical banking origination patterns. Regulatory requirements around customer identification, CDD, declarations, and recordkeeping make intake a distinct and durable business ability rather than merely a channel concern.
