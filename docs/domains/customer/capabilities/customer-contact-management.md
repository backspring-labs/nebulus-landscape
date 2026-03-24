---
id: cap.customer_contact_management
type: capability
name: Customer Contact Management
domain_id: domain.customer
status: draft
source_refs:
  - src.ffiec_bsaaml_cip
  - src.occ_red_flags_address_discrepancies
  - src.ecfr_glba_safeguards_16cfr314
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Contact Management

## 1. Definition

Customer Contact Management is the business ability to manage customer contact points (addresses, phone numbers, email addresses, and other addressability endpoints) including their type, validity, usability, effectivity, and history, so the institution can reliably and appropriately communicate with the customer.

This capability optimizes for "addressability": being able to reach the correct person via the correct channel with correct routing and evidence.

## 2. Business Outcome
- Reliable customer communications (servicing, compliance notices, marketing where permitted).
- Reduced operational failure (returned mail, unreachable customers, misstated delivery preferences).
- Lower fraud and account takeover exposure during contact changes and contact-point reuse.
- Improved customer experience by reducing repeated verification and "bounced" communications.

## 3. Scope

### Included activities
- Capture and maintain multiple contact points per customer with type semantics: address types (residential, mailing, legal, seasonal, employer address) with effective dates; phone number types (mobile, landline, work, VOIP, shared/family) with verification flags; email address types (primary, backup) with deliverability and verification status.
- Standardize and validate contact points (formatting, normalization, reference-data checks).
- Manage contact-point verification workflows (e.g., OTP verification for phone/email; address validation; return-mail feedback loops).
- Manage contact change history, effective-dating, and conflict resolution (primary vs secondary).
- Detect and route exceptions (e.g., undeliverable mail, repeated bounce, disconnected phone).
- Provide contact-point data products to downstream communications and servicing systems under appropriate governance.

### Excluded activities
- Choosing what content to send, UX flows, and channel UI (owned by domain.channels_experience).
- Marketing campaign orchestration and outbound communications execution (often owned by a Channels/Comms or Marketing capability; not Customer).
- Authentication/session proof of the customer during "change address" flows (owned by domain.identity_access).
- Fraud decisioning on whether a contact change is fraudulent (owned by domain.risk_fraud), although this capability can trigger signals and enforce step-up requirements.

## 4. Upstream Dependencies
- cap.customer_identity_establishment (contact points must attach to a stable customer identity).
- cap.customer_application_intake (initial capture of address/phone/email during onboarding).
- cap.customer_onboarding_orchestration (drives completion/verification milestones).
- domain.identity_access (step-up authentication to authorize sensitive contact changes).
- domain.risk_fraud (policy outputs for contact-change risk handling).
- domain.channels_experience (entry and presentation; error messages; customer self-service flows).

## 5. Downstream Dependencies
- cap.customer_preference_management (preferences act on contact points -- e.g., "contact me at X").
- cap.customer_consent_management (communications permissions depend on contact method and purpose).
- cap.customer_disclosure_attestation_management (deliver disclosures to correct contact address/e-delivery endpoint).
- domain.servicing (contact center needs accurate contact points and history).
- domain.accounts (statements/tax docs delivery often depends on accurate addresses/emails).
- domain.payments (payment confirmations and fraud alerts often depend on current phone/email).

## 6. Related Value Streams
- `vs.customer_record_establishment`
- `vs.customer_record_maintenance`
- `vs.customer_communications_management`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.update_contact_information`
- `journey.change_address`
- `journey.verify_contact_point`
- `journey.report_customer_unreachable`

## 8. Likely Processes
- proc.capture_contact_points
- proc.address_standardization_and_validation
- proc.contact_point_verification
- proc.sensitive_contact_change_workflow
- proc.return_mail_and_bounce_processing
- proc.contact_point_effectivity_management

## 9. Risks
- risk.contact_point_decay (contact points become invalid; customer becomes unreachable).
- risk.misdirected_communications (privacy breach from sending to wrong address/email/phone).
- risk.account_takeover_via_contact_change (attacker changes contact points to intercept OTPs/notices).
- risk.disclosure_delivery_failure (required notices/disclosures not delivered or not provable).
- risk.contact_data_inconsistency (different systems use different addresses; customer disputes).

## 10. Controls
- ctrl.address_validation_and_standardization (format + reference checks; reduce duplicates).
- ctrl.contact_point_effective_dating (clear rules for when changes take effect and supersession).
- ctrl.step_up_authentication_for_sensitive_changes (verify customer identity through domain.identity_access).
- ctrl.red_flags_contact_change_controls (holds, out-of-band verification, enhanced monitoring for high-risk changes).
- ctrl.return_mail_and_bounce_monitoring (flag for follow-up, servicing, or compliance handling).
- ctrl.audit_logging_for_contact_changes (tamper-evident change history).

## 11. Typical Systems/Providers

### Typical systems
- Customer contact data store (within CIF/CRM)
- Address standardization/validation service integration
- Contact verification/OTP service integration (shared with identity/channels)
- Return-mail processing and bounce-handling integrations
- Eventing platform for contact-change notifications

### Representative providers
- Address validation: Loqate, Melissa, Smarty, Experian (data quality services)
- Communications/verification: Twilio, Sinch
- CRM/contact hub: Salesforce, Microsoft Dynamics 365

## 12. Open Questions
- Should contact-point verification status be owned by Contact Management, or only referenced from identity_access artifacts?
- What contact change patterns should be "always step-up" vs "risk-based," and who owns the policy (Customer vs risk_fraud vs orchestration_control)?
- Should "contactability scores" and bounce rates be treated as Customer record attributes, or as analytics owned by data_rewards?

## 13. Provenance

Grounded in the Customer domain seed boundaries with emphasis on addressability and fraud/identity-theft implications of contact changes. Regulatory anchors include CIP identifying info relevance and identity theft red flags/address discrepancy expectations; security controls anchored in Safeguards concepts. Confidence is medium due to variation across institutions and jurisdictions.
