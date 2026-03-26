---
id: cap.account_interest_and_fee_configuration_management
type: capability
name: Account Interest and Fee Configuration Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Interest and Fee Configuration Management

## 1. Definition

Account Interest and Fee Configuration Management is the enduring business ability to configure and manage account-level interest accrual parameters, rate structures, fee schedules, and fee waiver rules. It owns the product configuration layer that defines what rates apply, what fees are charged, and under what conditions waivers are granted, while leaving the actual computation and posting of interest and fees to the ledger domain. This capability bridges product-level rate and fee definitions with the specific configuration applied to individual accounts.

## 2. Business Outcome

Ensure every account operates under correctly configured interest rate parameters and fee schedules that are consistent with the governing product definition, properly disclosed to the customer, and governed with appropriate change controls so that interest accrual and fee assessment downstream produce accurate, compliant, and intended financial outcomes.

## 3. Scope

### Included activities
- Configuring account-level interest rate tiers, accrual methods, and compounding parameters
- Managing account-level fee schedules including monthly maintenance fees, transaction fees, and service charges
- Configuring and governing fee waiver rules, eligibility criteria, and waiver limits
- Managing rate and fee changes with effective dating, customer notification, and disclosure compliance
- Validating account-level rate and fee configuration against product-level definitions to detect drift or inconsistency
- Supporting promotional or introductory rate and fee structures with expiration management
- Recording all rate and fee configuration changes for audit, compliance, and dispute resolution
- Coordinating with the ledger domain to ensure configuration changes are reflected in computation

### Excluded activities
- Interest accrual calculation, compounding execution, and interest posting (`domain.ledger_core`)
- Fee assessment execution and fee transaction posting (`domain.ledger_core`)
- Product-level rate table and fee schedule definition (`cap.deposit_account_product_management`)
- Pricing engine execution for real-time rate determination (`domain.pricing`)
- Regulatory rate ceiling or floor enforcement at the market level (`domain.compliance_legal`)
- Account opening and initial configuration assignment (`cap.account_opening`, `cap.account_terms_and_feature_management`)

## 4. Upstream Dependencies
- `cap.deposit_account_product_management` for product-level rate tables and fee schedule definitions that govern allowed configurations
- `cap.account_terms_and_feature_management` for the account's product assignment and feature context
- `cap.account_opening` for the initial account record to which rate and fee configurations are attached
- `domain.compliance_legal` for regulatory constraints on rates and fees (e.g., Regulation DD disclosure requirements, usury limits)

## 5. Downstream Dependencies
- `domain.ledger_core` reads rate and fee configuration to calculate interest accrual, assess fees, and post transactions
- `domain.pricing` uses account-level configuration for rate determination and fee calculation
- `domain.channels_experience` displays rate and fee information to customers and staff
- `domain.reporting_analytics` for rate and fee composition reporting, fee income analysis, waiver utilization metrics, and regulatory disclosure auditing
- `cap.account_statement_and_cycle_management` for including rate and fee disclosures in periodic statements

## 6. Related Value Streams
- `vs.account_feature_configuration`
- `vs.deposit_product_realization`
- `vs.fee_income_optimization`
- `vs.regulatory_compliance_assurance`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.update_account_features`
- `journey.review_account_fees`
- `journey.apply_fee_waiver`

## 8. Likely Processes
- Interest rate tier configuration and assignment
- Fee schedule configuration and assignment
- Fee waiver rule configuration and eligibility management
- Rate and fee change governance with effective dating and disclosure coordination
- Account-to-product rate and fee consistency validation
- Promotional rate and fee structure management with expiration tracking
- Rate and fee configuration change audit logging
- Periodic rate and fee configuration review and reconciliation

## 9. Risks
- Interest rate misconfiguration causes incorrect accrual amounts, creating financial loss or customer overcharges
- Fee schedule misconfiguration results in incorrect fee assessment, driving customer complaints and potential regulatory action
- Fee waiver abuse where waivers are granted outside policy without adequate governance, eroding fee income
- Rate changes not properly disclosed to customers before taking effect, violating Regulation DD requirements
- Configuration-vs-posting divergence where the rate and fee configuration in the account system does not match what the ledger system actually computes, producing incorrect financial results

## 10. Controls
- Rate configuration product consistency validation ensuring account-level rates remain within the bounds defined by the governing product
- Fee schedule product consistency validation ensuring account-level fees match the governing product's fee structure
- Fee waiver authorization and limits requiring approval for waivers above threshold amounts or outside standard eligibility criteria
- Rate change disclosure compliance ensuring customers receive required notice before rate or fee changes take effect
- Interest and fee configuration audit trail capturing every configuration change with who, when, what, and authorization reference

## 11. Typical Systems/Providers

### Typical systems
- Interest rate configuration service
- Fee schedule management service
- Fee waiver rules engine
- Pricing and rate configuration service
- Account parameter engine

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Thought Machine
- Mambu

## 12. Open Questions
- Should this capability remain in `domain.accounts` or shift partially to `domain.ledger_core` if fee/interest configuration is tightly coupled to posting logic in practice?
- How should promotional or temporary rate and fee structures interact with the standard configuration lifecycle?
- Where does the boundary sit between account-level fee configuration managed here and transaction-level fee determination in the ledger?
- Should fee waiver governance be a sub-capability given its distinct approval workflow and income impact?
- How should rate and fee configuration synchronization work when the account parameter engine and the ledger's computation engine are separate systems?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the configuration layer that governs interest and fee behavior at the account level. Informed by Regulation DD (Truth in Savings) for rate and fee disclosure obligations, Regulation Q for interest rate constraints, common core banking rate and fee parameter management patterns, and the explicit boundary decision that Accounts owns configuration while Ledger owns computation and posting. Confidence is high because interest and fee configuration is a well-understood capability with clear regulatory requirements, though the Accounts-vs-Ledger boundary for this capability requires ongoing attention as the Ledger domain is modeled.
