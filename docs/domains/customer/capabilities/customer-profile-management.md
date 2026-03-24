---
id: cap.customer_profile_management
type: capability
name: Customer Profile Management
domain_id: domain.customer
status: draft
source_refs:
  - src.ffiec_bsaaml_cip
  - src.ecfr_reg_p_12cfr1016
  - src.ecfr_glba_safeguards_16cfr314
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Profile Management

## 1. Definition

Customer Profile Management is the business ability to create, maintain, and govern the institution's core customer profile record and its business attributes over time. It ensures the institution has a consistent, current, auditable, and appropriately safeguarded representation of "who the customer is" at the relationship level.

This capability focuses on the customer profile as a governed business record (a source of truth for customer attributes and their history), not on channel UX, workflow execution, or identity proofing.

## 2. Business Outcome

A trusted, current, and usable customer profile that:
- enables compliant onboarding and ongoing servicing;
- reduces operational friction caused by inconsistent or missing customer attributes;
- supports downstream account-, servicing-, risk-, and communications-dependent processes;
- provides auditability and traceability for changes to customer attributes.

## 3. Scope

### Included activities
- Define the profile's canonical data model for Person/Customer and (where applicable) Business/Customer relationship profiles (e.g., customer type, residency flags, customer category, onboarding status attributes that belong to the customer record).
- Create initial customer profile attributes at or near "customer identity establishment," including attribute provenance (who/what supplied the attribute) and effective dating.
- Maintain profile attributes across lifecycle changes (name change, updated employment, residency changes, tax residency flags, changed legal capacity/guardian indicators, etc.).
- Manage profile attribute quality (validation rules, standardization, allowed values, completeness thresholds, stewardship workflows).
- Maintain change history and audit trails for profile updates.
- Provide profile reads/exports to authorized downstream consumers as a governed data product (within internal policy).

### Excluded activities
- Identity proofing execution, authentication, credential issuance, step-up authentication, session trust (owned by domain.identity_access).
- AML/KYC/fraud risk decisioning or sanctions decisions (owned by domain.risk_fraud).
- Account contract creation, account ownership/beneficiary structures within the account contract (owned by domain.accounts).
- Workflow engine execution and policy enforcement runtime (owned by domain.orchestration_control).
- UI presentation of profile fields and customer self-service UX (owned by domain.channels_experience).

## 4. Upstream Dependencies
- cap.customer_identity_establishment (creates the initial customer record identity container).
- cap.customer_party_resolution (determines whether a new profile should be created or linked/merged).
- cap.customer_application_intake (provides normalized applicant-supplied data to seed profile attributes).
- domain.identity_access (may provide verification status outputs and identity proofing outcomes as inputs to profile attributes).
- domain.risk_fraud (may provide risk ratings, CDD outcomes, or KYC refresh triggers as inputs, without transferring decision ownership).

## 5. Downstream Dependencies
- cap.customer_contact_management (often implemented as a related but distinct subdomain; profile must reference contact point records).
- cap.customer_preference_management (preferences depend on profile identity and segmentation context).
- cap.customer_consent_management (consents must link to a stable customer id and profile context).
- cap.customer_relationship_management (relationship context often uses profile type/status).
- domain.accounts (account opening and maintenance consume profile attributes; disputes arise when profile differs from account contract).
- domain.servicing (case handling uses profile attributes and change history).
- domain.data_rewards (analytics, segmentation inputs; must respect consent and privacy constraints).

## 6. Related Value Streams
- `vs.customer_record_establishment`
- `vs.customer_record_maintenance`
- `vs.privacy_and_permissions_governance`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.update_customer_profile`
- `journey.kyc_refresh`
- `journey.change_legal_name`
- `journey.convert_prospect_to_customer`

## 8. Likely Processes
- proc.create_customer_profile
- proc.profile_attribute_validation
- proc.profile_change_management
- proc.profile_stewardship_review
- proc.profile_data_correction
- proc.profile_audit_reporting

## 9. Risks
- risk.inaccurate_customer_profile (downstream compliance and operational failures due to wrong/obsolete attributes).
- risk.profile_data_fragmentation (multiple "sources of truth" diverge across systems).
- risk.unauthorized_profile_change (insider threat or compromised access leads to incorrect profile updates).
- risk.data_minimization_violation (collect/store profile attributes without a valid business/compliance basis).
- risk.customer_information_exposure (breach of customer information stored in profile systems).

## 10. Controls
- ctrl.profile_data_standards_and_dictionary (authoritative definitions, allowed values, and stewardship ownership).
- ctrl.validation_and_standardization_rules (format, range, completeness; documented and tested).
- ctrl.audit_logging_for_profile_changes (who/what/when/why; tamper-evident logs).
- ctrl.maker_checker_for_sensitive_changes (e.g., legal name, tax identifiers, residency).
- ctrl.role_based_access_and_least_privilege (write access tightly controlled; read access segmented).
- ctrl.safeguards_program_alignment (administrative/technical/physical safeguards; vendor oversight where applicable).
- ctrl.provenance_and_effective_dating (attribute source, time validity, and supersession logic).

## 11. Typical Systems/Providers

### Typical systems
- Customer master / customer information file (CIF)
- CRM or customer profile hub
- Master data management (MDM) hub and matching services (shared with party resolution)
- Data quality tooling / reference data services
- Audit log/eventing platform

### Representative providers
- Salesforce (Financial Services Cloud)
- Microsoft Dynamics 365
- Informatica MDM
- SAP Master Data Governance (MDG)
- Pega Customer Service / CRM patterns

## 12. Open Questions
- Should "verification status" fields (e.g., identity verified Y/N) live in the customer profile, or only as referenced evidence linked to domain.identity_access outcomes?
- What is the canonical boundary between profile attributes vs relationship context (segment/tier/household) to prevent "profile bloat"?
- How should profile attribute lifecycle tie to retention/recordkeeping rules and customer exit (Batch 3)?

## 13. Provenance

Synthesized from the nebulus-landscape Customer domain seed boundaries and common banking/fintech customer master patterns. Regulatory anchors include customer identification expectations (CIP), privacy notice/limitations concepts (Reg P), and GLBA Safeguards. Confidence is medium because implementation patterns vary materially by core banking architecture and jurisdiction.
