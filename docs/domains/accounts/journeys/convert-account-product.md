---
id: journey.convert_account_product
type: journey
name: Convert Account Product
domain_ids:
  - domain.accounts
  - domain.customer
  - domain.risk_fraud
  - domain.channels_experience
  - domain.orchestration_control
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
value_stream_id: vs.deposit_account_servicing
process_id: proc.account_conversion_v1
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Convert Account Product

## 1. Objective

Enable a customer or institution to convert an existing deposit account from one product type to another, adjusting terms, features, interest and fee configurations, and party roles while preserving account history and maintaining regulatory and operational continuity.

## 2. Primary Actor

Retail customer requesting a product change, or institutional actor initiating conversion due to product discontinuation, regulatory requirement, or customer eligibility change.

## 3. Trigger

- Customer requests a product change (e.g., upgrading from a basic checking to a premium checking product).
- Institution initiates conversion due to product discontinuation, portfolio migration, or regulatory change.
- Customer eligibility changes trigger an automatic or recommended product conversion.

## 4. Preconditions

- The account exists and is in an active or eligible state for conversion.
- The target product is available and the customer meets eligibility criteria.
- Conversion policies and mapping rules between source and target products are defined.
- The requesting party has authority to initiate the conversion.

## 5. Happy Path

### Stage 1 — Conversion request and eligibility assessment
The conversion request is received and the account is evaluated for conversion eligibility.

**Capabilities activated:** `cap.account_conversion_and_migration_management`, `cap.account_status_and_restriction_management`

The conversion request is captured with reason, source product, target product, and channel. The account status is confirmed as eligible for conversion. Product-level eligibility rules for the target product are evaluated. Any blocking conditions are identified.

### Stage 2 — Terms and feature comparison and mapping
The institution maps the current product terms and features to the target product, identifying changes, additions, and removals.

**Capabilities activated:** `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`

Current terms are documented. Target product terms are assembled. Differences are identified — new fees, changed interest rates, modified features, or removed capabilities. The conversion impact is prepared for customer disclosure.

### Stage 3 — Disclosure, consent, and attestation
Required disclosures for the new product are presented and consents are captured.

**Capabilities activated:** `cap.customer_consent_management`, `cap.customer_disclosure_attestation_management`

New product disclosures, fee schedules, and terms are presented. Customer acknowledgement and consent are captured. Any regulatory disclosures specific to the product change are recorded.

### Stage 4 — Party role and authorization adjustment
Party roles and authorizations are reviewed and adjusted for the target product if required.

**Capabilities activated:** `cap.account_party_role_management`

Party role eligibility is re-evaluated under the target product's rules. Role adjustments are made if the target product has different authority structures or limitations. Access and transactional authorities are updated.

### Stage 5 — Product conversion execution
The account product is formally converted, applying new terms, features, and configurations.

**Capabilities activated:** `cap.account_conversion_and_migration_management`, `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`, `cap.account_maintenance_management`

The product code is changed. New terms and features are applied. Interest and fee configurations are updated. Account history is preserved with conversion markers. The conversion effective date is recorded.

### Stage 6 — Status and restriction update
Account status and any product-specific restrictions are updated for the new product.

**Capabilities activated:** `cap.account_status_and_restriction_management`

Product-specific restrictions or capabilities are applied. Transaction limits or channel access may be adjusted per the new product. The account continues in an active state under the new product terms.

### Stage 7 — Completion and confirmation
The conversion is confirmed and the customer lifecycle is updated if applicable.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.account_conversion_and_migration_management`

Conversion completion is recorded. Confirmation is provided to the customer. If the conversion changes the customer's relationship tier or segment, the Customer domain is notified. Downstream servicing and treatment rules are updated for the new product.

## 6. Alternate Paths

- **Institution-initiated mass conversion** — The institution converts a portfolio of accounts due to product discontinuation, requiring batch processing with individual customer notification.
- **Downgrade conversion** — The customer moves from a premium to a basic product, potentially losing features or benefits.
- **Conversion with balance or fee impact** — The conversion changes fee structures in a way that requires prorated adjustments or fee waivers for the transition period.
- **Conversion requiring additional verification** — The target product has stricter eligibility requirements, triggering additional verification before conversion can complete.
- **Conversion from dormant product** — A dormant account on a discontinued product is reactivated and converted simultaneously.

## 7. Failure / Exception Paths

- The customer does not meet eligibility requirements for the target product.
- Required disclosures or consents are not completed.
- The target product is not available in the customer's jurisdiction or channel.
- Party role structures are incompatible with the target product.
- System or configuration issues prevent clean conversion execution.
- The customer declines the new terms after reviewing the conversion impact.
- Regulatory or compliance constraints block the specific conversion path.

## 8. Related Domains

- `domain.accounts` — owns conversion eligibility, terms mapping, product execution, status transition, and party role adjustment
- `domain.customer` — owns consent/disclosure management and customer lifecycle state updates
- `domain.risk_fraud` — may be involved if the target product has different risk requirements or if eligibility re-assessment is needed
- `domain.channels_experience` — owns conversion request presentation, comparison displays, and confirmation flows
- `domain.orchestration_control` — may coordinate multi-step or batch conversion workflows

## 9. Related Capabilities

- `cap.account_conversion_and_migration_management`
- `cap.account_terms_and_feature_management`
- `cap.account_interest_and_fee_configuration_management`
- `cap.account_status_and_restriction_management`
- `cap.account_party_role_management`
- `cap.account_maintenance_management`
- `cap.customer_consent_management`
- `cap.customer_disclosure_attestation_management`
- `cap.customer_lifecycle_state_management`

## 10. Value Stream Context

This journey sits inside `vs.deposit_account_servicing` as a mid-lifecycle transformation event. It represents a change in the product contract without closing and reopening the account, preserving continuity of history, party relationships, and regulatory standing.

## 11. Supporting Process Candidates

- `proc.account_conversion_v1`
- `proc.conversion_eligibility_assessment`
- `proc.terms_and_feature_mapping`
- `proc.conversion_disclosure_and_consent`
- `proc.product_conversion_execution`
- `proc.party_role_adjustment`
- `proc.conversion_confirmation`

## 12. Key Risks and Controls Encountered

### Key risks
- Conversion to a product the customer is not eligible for
- Loss of features or benefits without adequate disclosure
- Incorrect terms or fee application after conversion
- History discontinuity or loss of audit trail
- Mass conversion errors affecting a large portfolio
- Missing consent or disclosure for the new product
- Party role incompatibility with target product

### Key controls
- Eligibility validation against target product rules
- Terms comparison and impact disclosure requirement
- Consent capture before conversion execution
- Conversion audit trail with before/after snapshots
- Batch conversion validation and rollback capability
- Party role compatibility check
- Post-conversion verification of applied terms and features

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account management and product catalog)
- Product configuration engine
- Customer master / CIF
- Disclosure and consent management platform
- Case management system (for batch conversions and exceptions)
- Notification service

### Representative providers
- Fiserv
- Jack Henry
- Temenos
- FIS
- nCino

## 14. Open Questions

- Should mass/batch conversions be modeled as a separate institutional journey or as a variant of this customer-facing journey?
- How should conversion interact with statement cycles — should conversion always align to cycle boundaries?
- What is the appropriate handling of in-flight transactions during conversion execution?
- Should conversion from a discontinued product be combined with the Reactivate Dormant Account journey or remain separate?
- How should promotional or temporary product terms be handled when converting to or from promotional products?

## 15. Provenance

This journey is medium-confidence because while the core conversion flow is well understood, the complexity of terms mapping between products, mass conversion scenarios, and the interaction with statement cycles and in-flight transactions introduces variability that differs significantly across institutions and core banking platforms. It exercises the `cap.account_conversion_and_migration_management` capability as its primary Accounts-domain concern and demonstrates the distinction between a product change (preserving the account) and a close-and-reopen pattern.
