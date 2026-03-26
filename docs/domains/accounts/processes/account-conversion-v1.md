---
id: proc.account_conversion_v1
type: process
name: Account Conversion v1
journey_id: journey.convert_account_product
capability_ids:
  - cap.account_conversion_and_migration_management
  - cap.account_terms_and_feature_management
  - cap.account_interest_and_fee_configuration_management
  - cap.account_status_and_restriction_management
  - cap.account_party_role_management
  - cap.account_maintenance_management
  - cap.customer_consent_management
  - cap.customer_disclosure_attestation_management
  - cap.customer_lifecycle_state_management
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Conversion v1

## 1. Objective

Operationally execute the controlled sequence required to convert a deposit account from one product type to another by assessing conversion eligibility, mapping terms and features between source and target products, capturing required disclosures and consents, adjusting party roles if necessary, executing the product conversion, updating account status and restrictions, and confirming completion — all while preserving account history and maintaining regulatory and operational continuity.

## 2. Scope

This process covers:
- conversion request intake and eligibility assessment
- terms and feature comparison and mapping between source and target products
- disclosure delivery, consent capture, and attestation for the new product
- party role and authorization adjustment if required by the target product
- product conversion execution with terms, features, and configuration changes
- account status and restriction update for the new product
- conversion completion and customer lifecycle update

This process does **not** own:
- customer relationship management or onboarding (`domain.customer`)
- risk or fraud re-assessment decisioning (`domain.risk_fraud`)
- GL posting or balance restatement (`domain.ledger_core`)
- channel presentation of conversion flows (`domain.channels_experience`)
- batch orchestration tooling for mass conversions (`domain.orchestration_control`)

## 3. Trigger

A customer requests a product change (e.g., upgrading from basic checking to premium checking), the institution initiates conversion due to product discontinuation or portfolio migration, or a customer eligibility change triggers an automatic or recommended conversion.

## 4. Entry Conditions

- The account exists and is in an active or eligible state for conversion.
- The target product is available and the customer meets eligibility criteria.
- Conversion policies and mapping rules between source and target products are defined.
- The requesting party has authority to initiate the conversion.
- Product catalog, terms, and disclosure packages for the target product are current.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.account_conversion.conversion_request_and_eligibility` | Conversion Request and Eligibility | `domain.accounts` |
| `stage.account_conversion.terms_and_feature_comparison` | Terms and Feature Comparison | `domain.accounts` |
| `stage.account_conversion.disclosure_consent_and_attestation` | Disclosure, Consent, and Attestation | `domain.accounts` with `domain.customer` participation |
| `stage.account_conversion.party_role_and_authorization_adjustment` | Party Role and Authorization Adjustment | `domain.accounts` |
| `stage.account_conversion.product_conversion_execution` | Product Conversion Execution | `domain.accounts` |
| `stage.account_conversion.status_and_restriction_update` | Status and Restriction Update | `domain.accounts` |
| `stage.account_conversion.completion_and_confirmation` | Completion and Confirmation | `domain.accounts` with `domain.customer` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.account_conversion.conversion_request_and_eligibility`

**Description:** The conversion request is received and the account is evaluated for conversion eligibility against the target product's requirements and the institution's conversion policies.

**Capabilities activated:** `cap.account_conversion_and_migration_management`, `cap.account_status_and_restriction_management`

**Systems involved:** `sys.core_banking_platform`, `sys.product_configuration_engine`

**Decision points:**
- Is the account in a state eligible for conversion (active, not restricted or held)?
- Does the customer meet the target product's eligibility criteria?
- Is the source-to-target product conversion path defined and permitted?
- Are there blocking conditions (pending transactions, active holds, in-flight changes)?

**Risks:** `risk.conversion_to_ineligible_product`, `risk.conversion_path_not_defined`, `risk.conversion_during_active_hold_or_pending_transaction`

**Controls:** `ctrl.target_product_eligibility_validation`, `ctrl.conversion_path_policy_check`, `ctrl.blocking_condition_assessment`

**Evidence generated:** Conversion request record, source and target product identification, eligibility assessment outcome, blocking condition inventory

**Handoff:** If eligible, proceed to terms and feature comparison. If ineligible, route to exception handling or decline.

### Stage: `stage.account_conversion.terms_and_feature_comparison`

**Description:** The institution maps the current product's terms and features to the target product, identifying all changes, additions, and removals to prepare the conversion impact for customer disclosure.

**Capabilities activated:** `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`

**Systems involved:** `sys.product_configuration_engine`, `sys.core_banking_platform`

**Decision points:**
- What terms are changing (rates, fees, limits, features)?
- Are any current features being removed or restricted in the target product?
- Are new features or benefits being added?
- Are there prorated fee adjustments or transition-period considerations?

**Risks:** `risk.incomplete_terms_mapping`, `risk.undisclosed_feature_removal`, `risk.incorrect_rate_or_fee_mapping`

**Controls:** `ctrl.terms_comparison_and_impact_disclosure`, `ctrl.feature_change_completeness_check`, `ctrl.rate_and_fee_mapping_validation`

**Evidence generated:** Terms comparison document (before/after), feature change inventory, rate and fee mapping record, conversion impact summary for disclosure

**Handoff:** If terms comparison is complete, proceed to disclosure and consent. If mapping issues exist, route to exception handling.

### Stage: `stage.account_conversion.disclosure_consent_and_attestation`

**Description:** Required disclosures for the target product are presented, the conversion impact is communicated, and customer consents and attestations are captured.

**Capabilities activated:** `cap.customer_consent_management`, `cap.customer_disclosure_attestation_management`

**Systems involved:** `sys.disclosure_and_consent_management_platform`, `sys.core_banking_platform`

**Participating domains:** `domain.customer`

**Decision points:**
- Were all required target-product disclosures presented in the correct version?
- Was the conversion impact (fee changes, feature changes, rate changes) clearly communicated?
- Were required consents and attestations captured?
- Did the customer acknowledge and accept the new terms?

**Risks:** `risk.missing_consent_for_new_product`, `risk.disclosure_not_delivered_or_not_provable`, `risk.customer_not_informed_of_adverse_changes`

**Controls:** `ctrl.consent_capture_before_execution`, `ctrl.disclosure_version_and_delivery_validation`, `ctrl.conversion_impact_communication_confirmation`

**Evidence generated:** Disclosure delivery and acceptance records, consent records with timestamp/channel/purpose, attestation records, conversion impact acknowledgement

**Handoff:** If disclosures and consents are complete, proceed to party role adjustment. If incomplete or declined, hold or terminate conversion.

### Stage: `stage.account_conversion.party_role_and_authorization_adjustment`

**Description:** Party roles and authorizations are reviewed and adjusted if the target product has different authority structures, role requirements, or limitations.

**Capabilities activated:** `cap.account_party_role_management`

**Systems involved:** `sys.core_banking_platform`, `sys.customer_master`

**Decision points:**
- Does the target product have different party role requirements or restrictions?
- Are current party roles compatible with the target product?
- Do any roles need to be added, removed, or modified?
- Are transactional authorities changing under the new product?

**Risks:** `risk.party_role_incompatibility_with_target`, `risk.unauthorized_role_change_during_conversion`, `risk.lost_authorization_without_notification`

**Controls:** `ctrl.party_role_compatibility_check`, `ctrl.role_change_authorization`, `ctrl.role_change_notification`

**Evidence generated:** Party role compatibility assessment, role adjustment records, authorization change records, notification references

**Handoff:** If party roles are adjusted or confirmed compatible, proceed to conversion execution. If incompatible without resolution, route to exception handling.

### Stage: `stage.account_conversion.product_conversion_execution`

**Description:** The account product is formally converted — the product code is changed, new terms and features are applied, interest and fee configurations are updated, and account history is preserved with conversion markers.

**Capabilities activated:** `cap.account_conversion_and_migration_management`, `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`, `cap.account_maintenance_management`

**Systems involved:** `sys.core_banking_platform`, `sys.product_configuration_engine`

**Decision points:**
- Is the conversion execution package complete (new product code, terms, features, rates, fees)?
- Are before-state snapshots captured for audit purposes?
- Is the conversion effective immediately or scheduled?
- Are prorated adjustments required for the transition?

**Risks:** `risk.incorrect_terms_application_after_conversion`, `risk.history_discontinuity_or_audit_trail_loss`, `risk.partial_conversion_failure`, `risk.mass_conversion_error`

**Controls:** `ctrl.conversion_audit_trail_with_snapshots`, `ctrl.conversion_execution_validation`, `ctrl.batch_conversion_validation_and_rollback`, `ctrl.post_conversion_terms_verification`

**Evidence generated:** Before-state snapshot, product code change record, terms and feature application records, interest and fee configuration change records, conversion effective date, conversion marker in account history

**Handoff:** If conversion execution succeeds, proceed to status and restriction update. If execution fails, route to rollback or exception handling.

### Stage: `stage.account_conversion.status_and_restriction_update`

**Description:** Account status and any product-specific restrictions or capabilities are updated for the target product.

**Capabilities activated:** `cap.account_status_and_restriction_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Does the target product have different transaction limits or channel access rules?
- Are there product-specific restrictions that need to be applied or removed?
- Should the account remain fully active or enter a temporary transition state?

**Risks:** `risk.incorrect_restriction_application`, `risk.lost_capabilities_without_notification`, `risk.status_inconsistency_after_conversion`

**Controls:** `ctrl.restriction_update_validation`, `ctrl.capability_change_notification`, `ctrl.post_conversion_status_verification`

**Evidence generated:** Status update record, restriction change records, capability adjustment records

**Handoff:** If status and restrictions are updated, proceed to completion and confirmation.

### Stage: `stage.account_conversion.completion_and_confirmation`

**Description:** The conversion is confirmed, the customer is notified, and the Customer domain is updated if the conversion changes the customer's relationship tier, segment, or lifecycle treatment.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.account_conversion_and_migration_management`

**Systems involved:** `sys.core_banking_platform`, `sys.customer_master`, `sys.notification_service`

**Participating domains:** `domain.customer`

**Decision points:**
- Does the conversion change the customer's relationship tier or segment?
- Should the Customer domain update lifecycle treatment rules?
- Is confirmation notification required and through what channel?

**Risks:** `risk.customer_not_notified_of_conversion`, `risk.relationship_tier_not_updated`, `risk.downstream_treatment_rules_stale`

**Controls:** `ctrl.conversion_completion_confirmation`, `ctrl.customer_notification_delivery`, `ctrl.relationship_tier_evaluation_trigger`

**Evidence generated:** Conversion completion record, customer notification confirmation, relationship tier evaluation reference, downstream treatment update reference

**Handoff:** Process complete; the account continues in servicing under the new product terms.

## 7. Decision Points

Key decisions across the process:
- Conversion eligible vs ineligible
- Conversion path defined vs undefined
- Terms mapping complete vs incomplete
- Customer consent obtained vs declined
- Party roles compatible vs incompatible
- Conversion execution successful vs failed
- Relationship tier changed vs unchanged

## 8. Alternate Outcomes

- Institution-initiated mass conversion due to product discontinuation
- Downgrade conversion from premium to basic product
- Conversion with prorated fee adjustments for the transition period
- Conversion requiring additional verification for stricter target product
- Conversion from a dormant account on a discontinued product (combined with reactivation)
- Customer declines conversion after reviewing terms impact

## 9. Risks (process-level)

- Conversion to a product the customer is not eligible for
- Loss of features or benefits without adequate disclosure
- Incorrect terms, rate, or fee application after conversion
- Account history discontinuity or loss of audit trail
- Mass conversion errors affecting a large portfolio
- Missing consent or disclosure for the new product
- Party role incompatibility with the target product
- Downstream treatment rules not updated after conversion

## 10. Controls (process-level)

- Target product eligibility validation
- Terms comparison and impact disclosure requirement
- Consent capture before conversion execution
- Conversion audit trail with before/after snapshots
- Batch conversion validation and rollback capability
- Party role compatibility check
- Post-conversion verification of applied terms and features
- Customer notification and relationship tier evaluation

## 11. Evidence / Audit Considerations

This process generates a comprehensive audit trail:
- conversion request and eligibility assessment
- terms comparison and impact documentation
- disclosure delivery and consent evidence
- party role compatibility and adjustment records
- before-state snapshot and conversion execution records
- status and restriction update records
- completion confirmation and customer notification
- relationship tier evaluation reference

## 12. Systems Involved (summary)

- core banking platform (account management and product catalog)
- product configuration engine
- customer master / CIF
- disclosure and consent management platform
- case management system (for batch conversions and exceptions)
- notification service

## 13. Provider Participation

Representative providers often adjacent to this process:
- Fiserv, Jack Henry, Temenos, FIS, nCino

## 14. Open Questions

- Should mass/batch conversions be modeled as a separate process or as a variant path within this process?
- How should conversion interact with statement cycles — should conversion always align to cycle boundaries?
- What is the appropriate handling of in-flight transactions during conversion execution?
- Should conversion from a discontinued product be combined with the dormancy reactivation process or remain separate?
- How should promotional or temporary product terms be handled when converting to or from promotional products?
- What rollback capability is required if conversion execution partially fails?

## 15. Provenance

This process is medium-confidence because while the core conversion flow is well understood, the complexity of terms mapping between products, mass conversion scenarios, and the interaction with statement cycles and in-flight transactions introduces variability that differs significantly across institutions and core banking platforms. It exercises `cap.account_conversion_and_migration_management` as its primary Accounts-domain capability and demonstrates the distinction between a product change (preserving the account) and a close-and-reopen pattern. It pairs with the account opening and closure processes as part of the mid-lifecycle transformation set.
