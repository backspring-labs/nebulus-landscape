---
id: cap.account_overdraft_and_protection_management
type: capability
name: Account Overdraft and Protection Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Overdraft and Protection Management

## 1. Definition

Account Overdraft and Protection Management is the enduring business ability to configure overdraft settings, manage overdraft protection linkages, and govern overdraft-related product features. It owns the customer's overdraft election (opt-in/opt-out), coverage tier configuration, protection source account relationships, and the product-level overdraft rules that determine how overdraft situations are handled, while leaving the actual overdraft transaction decisioning and fee posting to downstream domains.

## 2. Business Outcome

Ensure overdraft settings accurately reflect the customer's documented election and the product's governing rules, that protection linkages are properly configured and validated, and that the institution's overdraft program complies with Regulation E opt-in requirements so that overdraft events downstream are handled consistently with customer expectations and regulatory obligations.

## 3. Scope

### Included activities
- Managing customer overdraft election (opt-in, opt-out) with Regulation E compliance
- Configuring overdraft coverage tiers and limits at the account level
- Establishing and managing overdraft protection linkages between source and protected accounts
- Validating overdraft protection source account eligibility and availability
- Managing overdraft product feature rules (courtesy pay, overdraft line of credit, linked account transfer)
- Recording overdraft election and configuration changes with full audit trail
- Publishing overdraft configuration events for downstream consumers
- Supporting customer self-service and staff-assisted overdraft election changes

### Excluded activities
- Overdraft transaction decisioning and authorization (`domain.transaction_operations` or `domain.payments`)
- Overdraft fee calculation and posting (`domain.ledger_core`)
- Overdraft protection transfer execution between linked accounts (`domain.payments`)
- Overdraft line of credit origination and management (`domain.lending`)
- Balance monitoring and insufficient funds detection (`domain.transaction_operations`)
- Channel UX for overdraft election and settings management (`domain.channels_experience`)

## 4. Upstream Dependencies
- `cap.deposit_account_product_management` for product-level overdraft rules, coverage options, and eligibility criteria
- `cap.account_terms_and_feature_management` for the account's product assignment and feature context
- `cap.account_opening` for the initial account record to which overdraft settings are attached
- `domain.compliance_legal` for Regulation E opt-in requirements and overdraft disclosure obligations

## 5. Downstream Dependencies
- `domain.transaction_operations` or `domain.payments` consumes overdraft configuration for transaction-level overdraft decisioning
- `domain.ledger_core` uses overdraft settings for fee calculation and posting
- `domain.channels_experience` displays overdraft settings, election status, and protection linkages to customers and staff
- `domain.reporting_analytics` for overdraft program metrics, election rates, fee income analysis, and regulatory compliance reporting
- `cap.account_statement_and_cycle_management` for including overdraft disclosures and fee summaries in periodic statements

## 6. Related Value Streams
- `vs.account_feature_configuration`
- `vs.overdraft_protection_enablement`
- `vs.deposit_account_servicing`
- `vs.regulatory_compliance_assurance`

## 7. Related Journeys
- `journey.set_up_overdraft_protection`
- `journey.change_overdraft_settings`
- `journey.link_overdraft_protection_source`
- `journey.opt_in_overdraft_coverage`
- `journey.opt_out_overdraft_coverage`

## 8. Likely Processes
- Overdraft election capture and Regulation E compliance validation
- Overdraft coverage tier assignment and configuration
- Overdraft protection source account linkage and eligibility validation
- Overdraft election change processing (opt-in to opt-out and vice versa)
- Overdraft protection linkage maintenance and periodic validation
- Overdraft configuration event publication for downstream consumers
- Overdraft election and configuration audit logging
- Periodic overdraft program review and compliance assessment

## 9. Risks
- Overdraft opt-in violation where customers are charged overdraft fees for ATM and one-time debit card transactions without a documented opt-in election, violating Regulation E
- Overdraft protection linkage failure where the linked source account is closed, restricted, or ineligible but the linkage is not updated, causing protection transfer failures
- Overdraft fee exposure beyond policy where configuration allows overdraft fees to accumulate beyond the institution's stated policy or customer expectations
- Overdraft coverage misconfiguration where coverage tiers or limits do not match the product definition or the customer's election
- Overdraft election not recorded where the customer's opt-in or opt-out decision is not properly captured, creating regulatory and dispute exposure

## 10. Controls
- Overdraft opt-in compliance validation ensuring Regulation E opt-in is documented before overdraft fees are assessed on covered transaction types
- Overdraft protection linkage verification confirming source account eligibility at configuration time and periodically thereafter
- Overdraft fee policy enforcement ensuring fee accumulation stays within product policy and daily/monthly caps
- Overdraft coverage configuration governance validating account-level settings against product-level rules
- Overdraft election audit trail capturing every election change with timestamp, channel, customer acknowledgment, and disclosure delivery confirmation

## 11. Typical Systems/Providers

### Typical systems
- Overdraft settings service
- Overdraft protection linkage engine
- Overdraft election management service
- Account parameter engine
- Overdraft disclosure delivery service

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Q2
- Mambu

## 12. Open Questions
- Should overdraft management be a sub-capability of `cap.account_terms_and_feature_management` given its close relationship to account feature configuration, or does its distinct regulatory profile (Regulation E) justify standalone capability status?
- Where does the boundary sit between overdraft configuration managed here and overdraft decisioning managed in `domain.transaction_operations` or `domain.payments`?
- How should overdraft protection linkages interact with account closure or restriction when the source account is affected?
- Should overdraft line of credit configuration be managed here or in `domain.lending` when the protection source is a credit product?
- How should this capability handle overdraft program changes that require mass re-election or re-disclosure to existing customers?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on overdraft product feature configuration and protection management as a distinct capability within the deposit account domain. Informed by Regulation E (Electronic Fund Transfers) for overdraft opt-in requirements, Regulation DD (Truth in Savings) for overdraft fee disclosure obligations, CFPB overdraft guidance, and common core banking overdraft configuration patterns in US retail banking. Confidence is medium due to the boundary tension between overdraft configuration (owned here), overdraft decisioning (owned downstream), and the open question of whether this capability should be standalone or a sub-capability of account terms and feature management.
