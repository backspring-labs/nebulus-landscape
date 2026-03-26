---
id: cap.account_terms_and_feature_management
type: capability
name: Account Terms and Feature Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Terms and Feature Management

## 1. Definition

Account Terms and Feature Management is the enduring business ability to maintain account-level terms, pricing/fee features, interest features, overdraft options, and product-level options that are attached to the account contract. It bridges the gap between product-level definitions and the actual configuration applied to a specific account, governing how product rules are instantiated, customized within allowed parameters, and maintained over the account's life.

## 2. Business Outcome

Ensure every account operates under correctly instantiated, product-consistent, and governed terms and features so that interest, fees, limits, and options behave as the customer agreed to and the institution intended, with full traceability from product definition through account-level configuration.

## 3. Scope

### Included activities
- Instantiating product-level terms and features onto a new account at opening
- Maintaining account-level pricing, fee, interest, and limit configurations
- Managing overdraft option selection and configuration (opt-in/opt-out, coverage levels)
- Governing account feature changes with approval workflows and effective dating
- Validating account configuration against product rules to detect drift or inconsistency
- Managing account-level exceptions or overrides within governance bounds
- Supporting product conversion by remapping terms and features to a new product definition
- Recording feature change history for audit, compliance, and dispute resolution

### Excluded activities
- Product-level definition, versioning, and catalog management (`cap.deposit_account_product_management`)
- Interest accrual calculation, fee assessment execution, or transaction posting (`domain.transaction_operations`)
- Pricing engine execution for real-time rate determination (`domain.pricing`)
- Account opening and record creation (`cap.account_opening`)
- Disclosure presentment and contract acceptance (`cap.account_application_and_contract_formation`)
- Regulatory rate ceiling or floor enforcement (`domain.compliance_legal`)

## 4. Upstream Dependencies
- `cap.deposit_account_product_management` for the product definition and rule package that governs allowed terms and features
- `cap.account_opening` for the account record to which terms and features are attached
- `cap.account_application_and_contract_formation` for the accepted terms that define the initial feature configuration
- `domain.compliance_legal` for regulatory constraints on features (e.g., Regulation E overdraft opt-in requirements)

## 5. Downstream Dependencies
- `domain.transaction_operations` reads account terms and features to calculate interest, assess fees, and enforce limits
- `domain.pricing` uses account feature configuration for rate determination
- `domain.channels_experience` displays account features and allows customer-initiated changes within allowed parameters
- `domain.reporting_analytics` for terms and feature composition reporting, fee income analysis, and product utilization metrics
- `cap.account_funding_readiness_management` may reference feature state for funding eligibility

## 6. Related Value Streams
- `vs.account_contract_establishment`
- `vs.account_feature_configuration`
- `vs.deposit_product_realization`
- `vs.account_maintenance`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.update_account_features`
- `journey.change_overdraft_settings`
- `journey.convert_account_product`

## 8. Likely Processes
- Account terms instantiation from product definition at opening
- Account feature change request and governance
- Overdraft option management (opt-in, opt-out, coverage changes)
- Account-to-product configuration validation and drift detection
- Product conversion terms remapping
- Account feature exception and override governance
- Feature change effective dating and history recording
- Periodic account configuration review and reconciliation

## 9. Risks
- Account terms do not match the product definition, causing incorrect interest, fee, or limit behavior
- Feature configuration inconsistency across related accounts or bundled products
- Ungoverned terms drift where manual overrides accumulate without periodic review or reconciliation
- Downstream systems interpret feature configuration differently, causing calculation or display discrepancies
- Overdraft or fee misconfiguration creates regulatory exposure (e.g., Regulation E opt-in violations)

## 10. Controls
- Account-to-product configuration validation ensuring account terms remain consistent with the governing product definition
- Account terms effective dating with no retroactive changes without explicit governance and customer notification
- Feature change governance requiring approval for changes outside customer self-service parameters
- Rate, fee, and feature consistency checks comparing account configuration against product rules at scheduled intervals
- Account feature audit trail capturing every configuration change with who, when, what, and authorization reference

## 11. Typical Systems/Providers

### Typical systems
- Account parameter engine
- Product feature rules repository
- Pricing and rate configuration service
- Overdraft settings service
- Account maintenance workbench

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Mambu
- Thought Machine

## 12. Open Questions
- Should account-level overrides and exceptions be managed within this capability or in a dedicated exception governance capability?
- How should feature configuration handle promotional or temporary rate/fee structures that expire?
- Where does the boundary sit between account-level feature management and transaction-level feature interpretation?
- Should overdraft management be a sub-capability given its distinct regulatory requirements under Regulation E?
- How should feature configuration synchronization work across systems when the account parameter engine is not the sole source of truth?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the account-level instantiation of product terms and features. Informed by Regulation DD (Truth in Savings) for disclosure of account terms, Regulation E for overdraft opt-in requirements, and common core banking parameter management patterns in US retail and commercial banking. Boundary placement separates account-level feature management from both upstream product definition and downstream transaction execution.
