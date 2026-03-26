---
id: cap.deposit_account_product_management
type: capability
name: Deposit Account Product Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Deposit Account Product Management

## 1. Definition

Deposit Account Product Management is the enduring business ability to define, configure, version, and govern deposit-account product constructs — including checking, savings, money market, and certificate of deposit offerings — along with their feature sets, eligibility attributes, disclosure mappings, pricing/rate structures, and rule packages. It ensures that every product available for sale or servicing is fully specified, internally consistent, and traceable to its regulatory and business requirements before it reaches account opening or servicing processes.

## 2. Business Outcome

Ensure every deposit product the institution offers is fully defined, internally consistent, correctly mapped to disclosures and pricing, and governed through its lifecycle so that downstream account opening, servicing, and reporting systems operate from a single trusted product definition.

## 3. Scope

### Included activities
- Defining deposit product types, subtypes, and product families
- Configuring product feature sets including interest, fee, limit, and overdraft attributes
- Managing product eligibility rules and customer/channel qualification criteria
- Mapping products to required disclosures, agreements, and regulatory packages
- Versioning product definitions with effective dating and change governance
- Managing product retirement, sunset, and grandfathering rules
- Governing product-to-ledger and product-to-GL mapping definitions
- Coordinating product consistency across channels and origination platforms
- Maintaining the product catalog as the authoritative source for downstream consumers

### Excluded activities
- Account-level terms instantiation and per-account feature configuration (`cap.account_terms_and_feature_management`)
- Account opening orchestration and contract creation (`cap.account_opening`)
- Disclosure content authoring and legal document management (`domain.compliance_legal`)
- Pricing engine execution for real-time rate calculation (`domain.pricing`)
- Interest accrual, fee assessment, or transaction posting (`domain.transaction_operations`)
- Channel UX, product comparison tools, and marketing presentation (`domain.channels_experience`)

## 4. Upstream Dependencies
- Regulatory and compliance requirements from `domain.compliance_legal` for disclosure and feature constraints
- Enterprise reference data standards for product taxonomies and classification schemes
- Pricing and rate policy guidance from treasury or ALM functions
- Business strategy inputs for product portfolio direction and competitive positioning

## 5. Downstream Dependencies
- `cap.account_opening` consumes product definitions to determine what can be opened and under what conditions
- `cap.account_application_and_contract_formation` uses product-to-disclosure mappings to present the correct agreements
- `cap.account_terms_and_feature_management` instantiates product-level definitions into account-level terms
- `domain.transaction_operations` relies on product rules for interest calculation, fee assessment, and limit enforcement
- `domain.channels_experience` consumes product catalog data for display, comparison, and eligibility filtering

## 6. Related Value Streams
- `vs.deposit_product_governance`
- `vs.deposit_account_acquisition`
- `vs.product_lifecycle_management`
- `vs.account_contract_establishment`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.select_deposit_product`
- `journey.convert_account_product`
- `journey.launch_new_deposit_product`

## 8. Likely Processes
- Deposit product definition and feature specification
- Product change governance and version approval
- Product retirement and grandfathering execution
- Product-to-disclosure mapping maintenance
- Product catalog publishing and downstream notification
- Cross-channel product consistency review
- Product rule package testing and validation

## 9. Risks
- Product rule inconsistency across channels or origination paths causes different customers to receive different terms for the same product
- Product-to-disclosure misalignment means customers receive incorrect or outdated regulatory disclosures at account opening
- Retired products remain available for opening due to incomplete sunset execution
- Pricing or rate misconfiguration in the product definition propagates errors into every account opened under that product
- Unclear product-to-ledger mapping creates reconciliation and reporting failures downstream

## 10. Controls
- Product rule package governance requiring approval before activation
- Product version effective dating with no retroactive changes to active accounts without explicit migration
- Product-to-disclosure mapping validation before any product version goes live
- Cross-channel product consistency review as part of release governance
- Product-to-operating-model traceability ensuring every product maps to supported processes, systems, and GL structures

## 11. Typical Systems/Providers

### Typical systems
- Deposit product catalog
- Product parameter repository
- Pricing and rate repository
- Disclosure package repository
- Reference data management platform

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Mambu
- Thought Machine

## 12. Open Questions
- Should product-to-GL mapping governance live here or in a dedicated finance/ledger domain capability?
- How should product versioning interact with in-flight account opening sessions that started under a prior version?
- Where does competitive product benchmarking and market analysis sit relative to this capability?
- Should product eligibility rules that depend on customer risk tier or segment be co-owned with customer or risk domains?
- How should product catalog publishing handle channel-specific availability windows or regional restrictions?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on deposit-account product constructs. Informed by Regulation DD (Truth in Savings) disclosure requirements, core banking product configuration patterns, and common product governance operating models in US retail and commercial banking. Boundary placement distinguishes product definition governance from account-level instantiation, pricing execution, and disclosure content authoring.
