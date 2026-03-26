---
id: cap.account_conversion_and_migration_management
type: capability
name: Account Conversion and Migration Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Conversion and Migration Management

## 1. Definition

Account Conversion and Migration Management is the enduring business ability to plan, validate, and execute the conversion of accounts from one product type to another or the migration of accounts between core banking platforms. It governs eligibility assessment, terms mapping and re-pricing, data transformation and migration, regulatory disclosure of changes to account terms, customer communication, post-conversion reconciliation, and rollback execution when conversion or migration does not complete successfully.

## 2. Business Outcome

Ensure account product conversions and platform migrations are executed with full continuity of service, preservation of account history, accurate re-pricing and re-terming, proper regulatory disclosure of all changes to account terms and conditions, and minimal disruption to customer access and transaction processing.

## 3. Scope

### Included activities
- Assessing account eligibility for product conversion or platform migration based on account attributes, balances, and status
- Mapping account terms, features, and pricing from source product or platform to target product or platform
- Generating and delivering regulatory disclosures for changes to account terms resulting from conversion (Regulation DD)
- Executing account data transformation and migration including balances, transaction history, and account attributes
- Validating data integrity post-conversion through automated reconciliation of key account attributes and balances
- Managing customer communication and opt-in/opt-out workflows for voluntary conversions
- Executing rollback procedures when conversion or migration fails validation checks
- Coordinating with downstream systems to ensure continuity of payment instruments, recurring transactions, and channel access

### Excluded activities
- Routine account attribute changes that do not constitute a product conversion (`cap.account_maintenance_management`)
- Account terms and feature configuration for the target product (`cap.account_terms_and_feature_management`)
- New account opening when conversion requires closing and re-opening (`cap.account_opening`, `cap.account_closure_management`)
- Core banking platform selection and vendor management (`domain.technology`)
- Channel UX for conversion acceptance and post-conversion onboarding (`domain.channels_experience`)

## 4. Upstream Dependencies
- `cap.account_terms_and_feature_management` for target product terms, features, and pricing to map against
- `cap.account_status_and_restriction_management` for current account status and any restrictions that may block conversion
- `cap.account_interest_and_fee_configuration_management` for current and target interest and fee configurations
- `domain.product` for product catalog and eligibility rules governing which conversions are permitted
- `domain.customer` for customer consent and communication preferences for conversion notifications

## 5. Downstream Dependencies
- `cap.account_statement_and_cycle_management` for statement cycle adjustments required by product conversion
- `domain.payments` for payment instrument re-issuance and recurring payment continuity after conversion
- `domain.channels_experience` for post-conversion account display, feature access, and customer communication
- `domain.finance` for general ledger reclassification entries resulting from product type changes
- `domain.reporting_analytics` for conversion volume tracking, success rates, and post-conversion customer impact metrics

## 6. Related Value Streams
- `vs.account_lifecycle_management`
- `vs.deposit_account_servicing`
- `vs.product_optimization`
- `vs.platform_modernization`

## 7. Related Journeys
- `journey.convert_account_product_type`
- `journey.migrate_account_to_new_platform`
- `journey.accept_conversion_terms`
- `journey.review_post_conversion_account`

## 8. Likely Processes
- Conversion eligibility assessment and blocker identification
- Terms mapping and re-pricing from source to target product
- Regulatory disclosure generation and delivery for terms changes
- Customer communication and consent management for voluntary conversions
- Account data transformation and migration execution
- Post-conversion data integrity validation and reconciliation
- Downstream system notification and continuity verification
- Conversion rollback execution and customer notification on failure

## 9. Risks
- Conversion data loss or corruption during data transformation and migration, resulting in inaccurate account records or missing transaction history
- Terms mapping errors where source product features or pricing do not correctly translate to the target product, resulting in customer harm or financial loss
- Inadequate conversion disclosure failing to notify customers of material changes to account terms as required by Regulation DD
- Post-conversion service disruption where payment instruments, recurring transactions, or channel access fail to function correctly after conversion
- Conversion rollback failure where an unsuccessful conversion cannot be cleanly reversed, leaving accounts in an inconsistent state

## 10. Controls
- Conversion data integrity validation comparing key account attributes, balances, and transaction counts between source and target before and after conversion
- Conversion terms mapping review requiring documented verification that all source product terms correctly map to target product terms
- Conversion disclosure compliance check validating that all material terms changes are disclosed per Regulation DD requirements before conversion execution
- Post-conversion reconciliation verification confirming balances, transaction history, payment instruments, and channel access are intact after conversion
- Conversion rollback capability testing validating that rollback procedures restore accounts to their pre-conversion state without data loss

## 11. Typical Systems/Providers

### Typical systems
- Account conversion orchestrator
- Data migration engine
- Conversion terms mapper
- Conversion disclosure generator
- Conversion reconciliation service

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Infosys Finacle
- Tata Consultancy Services

## 12. Open Questions
- Should account conversion always be modeled as an in-place transformation, or should some conversions be modeled as close-and-reopen with history migration?
- How should conversion management interact with accounts that are part of a relationship package where conversion of one account affects the terms of others?
- Where does the boundary sit between a product feature change managed by `cap.account_terms_and_feature_management` and a full product conversion managed here?
- How should platform migration differ from product conversion in orchestration, given that migration typically involves all accounts rather than selective conversion?
- Should conversion rollback have a time window, or should reversal be possible indefinitely after conversion?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on account product conversion and platform migration patterns in US retail banking. Informed by Regulation DD (Truth in Savings) for disclosure obligations triggered by changes to account terms, common core banking conversion patterns, and operational challenges observed in large-scale platform migrations. Confidence is medium because while product conversion is a recognized capability, its boundaries with account maintenance, terms management, and account opening/closure vary significantly across institutions, and large-scale platform migration introduces additional complexity that may warrant separate treatment.
