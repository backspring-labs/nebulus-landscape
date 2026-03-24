---
id: cap.customer_consent_management
type: capability
name: Customer Consent Management
domain_id: domain.customer
status: draft
source_refs:
  - src.ecfr_reg_p_12cfr1016
  - src.ecfr_reg_v_affiliate_marketing_12cfr1022_subpartc
  - src.ftc_can_spam_guide
  - src.fcc_tcpa_rules
  - src.ecfr_tsr_16cfr310
  - src.cfpb_pfdr_12cfr1033
  - src.cfpb_pfdr_reconsideration_fedreg
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Consent Management

## 1. Definition

Customer Consent Management is the business ability to capture, manage, evidence, and revoke legally meaningful permissions, opt-ins, opt-outs, and data-sharing authorizations granted (or withdrawn) by customers.

It focuses on the "permission record" and its governance: what the institution is permitted to do, for what purpose, for what data, through what channel(s), for what duration, and how it proves that permission.

## 2. Business Outcome
- Lawful, provable execution of customer permissions and opt-outs.
- Reduced compliance risk from unauthorized data sharing or prohibited communications.
- Faster, safer enablement of new data-sharing and personalization use cases by reusing a governed consent record.
- Improved customer trust through transparent controls and self-service revocation.

## 3. Scope

### Included activities
- Capture and manage consent artifacts with structured scope: purpose(s), data categories, recipients/third parties, channels, effective time window, expiration/renewal requirements; source and method of capture (customer self-service, agent-assisted, partner channel); evidence references (clickthrough, call recording reference, signed form reference, e-sign record id).
- Manage consent lifecycle: create, update, suspend, revoke, expire, renew; enforce precedence rules across overlapping consents (e.g., product consent overrides? global opt-out overrides?).
- Maintain auditability: immutable consent history; current effective consent snapshot; reporting.
- Publish consent status to downstream consumers for enforcement (as a governed interface).
- Manage legally required opt-outs and suppressions when they are permissions constructs (e.g., privacy opt-out, affiliate marketing opt-out).

### Excluded activities
- Delivering and evidencing regulated disclosures and agreements (owned by cap.customer_disclosure_attestation_management), though consent management may store the permissions related to e-delivery as a legal consent artifact depending on the institution's interpretation.
- Real-time policy enforcement engines and orchestration runtime (owned by domain.orchestration_control).
- Identity proofing, authentication, and authorization to change consent settings (owned by domain.identity_access).
- Third-party risk decisioning and fraud/AML decisions regarding whether a third party should be allowed (owned by domain.risk_fraud), though consent can be an input.

## 4. Upstream Dependencies
- cap.customer_profile_management (consents must attach to a stable customer identity and profile).
- cap.customer_contact_management (channel consents depend on valid contact points).
- cap.customer_application_intake (captures initial consents during onboarding).
- cap.customer_onboarding_orchestration (drives completion of consent milestones).
- domain.channels_experience (consent UX; transparency; dark-pattern avoidance).
- domain.identity_access (authn/authz and step-up for sensitive consent changes).

## 5. Downstream Dependencies
- cap.customer_preference_management (preferences are bounded by consents/opt-outs).
- cap.customer_disclosure_attestation_management (some disclosures require consent to e-deliver; consent linkage required).
- domain.data_rewards (segmentation/personalization must check consent scope and permitted uses).
- domain.channels_experience (feature enablement depends on permission state).
- domain.accounts (some product features and delivery modes require e-delivery consent and/or opt-outs).
- domain.risk_fraud (risk monitoring uses consent context; e.g., data-sharing authorizations can become fraud vectors).

## 6. Related Value Streams
- `vs.privacy_and_permissions_governance`
- `vs.customer_communications_management`
- `vs.open_banking_data_sharing`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.privacy_opt_out`
- `journey.affiliate_marketing_opt_out`
- `journey.manage_marketing_consent`
- `journey.share_data_with_third_party`
- `journey.revoke_data_sharing_consent`
- `journey.update_data_sharing_permissions`

## 8. Likely Processes
- proc.consent_capture
- proc.consent_scope_management
- proc.consent_evidence_management
- proc.consent_revocation_and_withdrawal
- proc.consent_propagation_to_enforcement_points
- proc.consent_audit_and_reporting

## 9. Risks
- risk.invalid_or_missing_consent (cannot demonstrate permission when challenged).
- risk.consent_not_honored (opt-out ignored; communications or sharing continues).
- risk.overbroad_consent_scope (purpose not specific; improper reuse of data).
- risk.dark_pattern_or_deceptive_capture (consent capture UX considered unfair/deceptive).
- risk.unauthorized_data_sharing (third parties receive data without valid authorization).
- risk.consent_fragmentation (multiple systems store conflicting opt-out decisions).

## 10. Controls
- ctrl.central_consent_registry (one authoritative record and API/feeds for downstream enforcement).
- ctrl.consent_granularity_and_purpose_limitation (structured scope; "why" and "what data" explicit).
- ctrl.consent_evidence_and_audit_trail (immutable history; time stamps; evidence references).
- ctrl.revocation_propagation_sla (timely distribution of revocation to all enforcement points).
- ctrl.separation_of_preference_vs_consent (prevent preference systems from overriding legal opt-out).
- ctrl.privacy_notice_and_opt_out_controls (privacy notices, opt-out mechanisms, and delivery requirements tracking).
- ctrl.affiliate_marketing_opt_out_controls (use of affiliate eligibility info constrained; opt-out maintained).
- ctrl.communications_consent_controls (channel-specific permissioning for texts/calls where required).

## 11. Typical Systems/Providers

### Typical systems
- Consent registry / consent ledger
- Preference center (integrated; separate capability but often implemented jointly)
- Data-sharing authorization service (open banking / third-party access)
- Marketing suppression/suppression list services integrated with consent

### Representative providers
- Dedicated consent/privacy platforms: OneTrust, TrustArc, Ketch, Usercentrics
- Open banking/data access platforms (varies): internal API gateways + authorization service; fintech aggregators for third-party access

## 12. Open Questions
- For e-delivery consent: should it live only in Disclosure/Attestation Management, or be dual-tracked (E-SIGN "consent" in consent ledger + evidence in disclosure/attestation)?
- How should the model represent 1033-authorized third parties and scopes while the regulatory landscape remains in reconsideration and affected by litigation?
- When international (GDPR/UK GDPR) applies, should the repo model consent as one unified construct with jurisdictional constraints, or separate capability variants?

## 13. Provenance

Built from banking privacy and marketing permissioning patterns and from explicit regulatory frameworks for privacy notice/opt-out, affiliate marketing opt-out, and permissioned data access concepts. Confidence is medium because consent scope granularity and jurisdictional treatment vary, and U.S. open-banking timelines are time-sensitive.
