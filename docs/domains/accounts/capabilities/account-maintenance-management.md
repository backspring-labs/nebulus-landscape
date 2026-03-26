---
id: cap.account_maintenance_management
type: capability
name: Account Maintenance Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Maintenance Management

## 1. Definition

Account Maintenance Management is the enduring business ability to perform routine and non-routine changes to account attributes throughout the account lifecycle, including address of record, account nickname, delivery preferences, product feature changes, and other account-level modifications. It governs the request, authorization, execution, propagation, and audit of changes to account data that do not constitute a status transition, hold, or product conversion.

## 2. Business Outcome

Ensure account attributes remain accurate, current, and consistent across all systems and channels so that customer communications reach the right destination, account configurations reflect the customer's current preferences, and every change is authorized, traceable, and propagated to all downstream consumers without data quality degradation.

## 3. Scope

### Included activities
- Processing routine account attribute changes (address, nickname, delivery preferences, contact method updates)
- Processing non-routine account changes (product feature modifications, special handling instructions)
- Governing maintenance request authorization based on change type, channel, and risk level
- Propagating account changes to downstream systems, channels, and service providers
- Validating changed data against quality rules (e.g., address validation, format checks)
- Managing bulk maintenance operations with appropriate governance and review
- Recording all maintenance changes with full audit trail including who, when, what, and authorization reference
- Coordinating with customer domain for changes that affect both customer and account records (e.g., address changes)

### Excluded activities
- Account status transitions and restriction management (`cap.account_status_and_restriction_management`)
- Account terms, features, and pricing configuration (`cap.account_terms_and_feature_management`)
- Account product conversion or migration (`cap.account_conversion_and_migration_management`)
- Customer-level profile changes that do not directly affect account attributes (`domain.customer`)
- Channel UX for maintenance request capture and presentation (`domain.channels_experience`)
- Account opening and initial attribute assignment (`cap.account_opening`)

## 4. Upstream Dependencies
- `cap.account_opening` for the account record whose attributes are being maintained
- `cap.account_terms_and_feature_management` for product rules constraining which attributes can be changed
- `domain.customer` for customer-level data that may need coordinated updates (e.g., address change affecting all accounts)
- `domain.identity_access` for authenticated and authorized access context governing who can make changes
- `domain.channels_experience` for the channel entry points through which maintenance requests are initiated

## 5. Downstream Dependencies
- `domain.channels_experience` consumes updated account attributes for display and interaction
- `domain.payments` consumes account attribute changes that affect payment routing or processing
- `domain.servicing` for maintenance request tracking and exception handling
- `domain.reporting_analytics` for maintenance volume reporting, data quality metrics, and change pattern analysis
- `cap.account_statement_and_cycle_management` consumes address and delivery preference changes for statement delivery

## 6. Related Value Streams
- `vs.account_maintenance`
- `vs.account_servicing_excellence`
- `vs.deposit_account_servicing`
- `vs.customer_data_quality`

## 7. Related Journeys
- `journey.update_account_preferences`
- `journey.change_account_product_type`
- `journey.update_account_address`
- `journey.update_account_delivery_preferences`
- `journey.correct_account_data_error`

## 8. Likely Processes
- Account attribute change request intake and validation
- Maintenance request authorization and approval workflow
- Account attribute change execution and system update
- Change propagation to downstream systems and channels
- Address change validation and coordination with customer domain
- Bulk maintenance request governance and execution
- Maintenance change audit logging and reporting
- Maintenance exception capture and resolution

## 9. Risks
- Unauthorized account changes made without proper authentication or authorization, potentially enabling fraud or social engineering attacks
- Account attribute data quality degradation from unvalidated changes accumulating over time
- Maintenance changes not propagated to all downstream systems, creating data inconsistency across channels and services
- Address change fraud where bad actors redirect communications (statements, cards, notices) to unauthorized addresses
- Ungoverned bulk maintenance operations introducing widespread data quality issues without adequate review or rollback capability

## 10. Controls
- Account maintenance authorization matrix defining required authentication and approval levels by change type and channel
- Account change propagation validation confirming all downstream systems received and applied the change
- Address change verification through out-of-band confirmation or secondary authentication for high-risk address changes
- Maintenance change audit trail capturing every attribute change with before/after values, timestamp, channel, and authorizing identity
- Bulk maintenance governance requiring pre-execution review, approval, and post-execution reconciliation for batch changes

## 11. Typical Systems/Providers

### Typical systems
- Account maintenance workbench
- Account attribute change service
- Address validation service
- Maintenance request queue
- Account change event publisher

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Q2
- Alkami

## 12. Open Questions
- Should account maintenance own address changes end-to-end, or should address changes be coordinated by `domain.customer` with account maintenance as a participant?
- How should maintenance governance differ between customer self-service changes and staff-initiated changes?
- Where does the boundary sit between routine maintenance managed here and feature/terms changes managed by `cap.account_terms_and_feature_management`?
- Should maintenance request tracking and exception handling live here or in `domain.servicing`?
- How should maintenance changes be handled for accounts with multiple owners who may have conflicting change requests?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the ongoing maintenance of account attributes throughout the account lifecycle. Informed by Regulation DD (Truth in Savings) for disclosure obligations triggered by account changes, common core banking maintenance patterns, and operational challenges observed in change propagation, address change fraud prevention, and bulk maintenance governance in US retail banking. Confidence is high because account maintenance is a well-established, high-volume operational capability with clear patterns across all deposit account platforms.
