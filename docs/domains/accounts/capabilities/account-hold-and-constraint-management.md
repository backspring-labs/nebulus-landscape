---
id: cap.account_hold_and_constraint_management
type: capability
name: Account Hold and Constraint Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Hold and Constraint Management

## 1. Definition

Account Hold and Constraint Management is the enduring business ability to apply, manage, and release holds, blocks, levies, garnishments, and other constraints on accounts. It governs the specific encumbrances that restrict account usage, affect available balances, or limit permitted transactions without necessarily changing the account's lifecycle state. This capability manages the constraint lifecycle from application through release, including priority, stacking, expiration, and compliance with legal and regulatory requirements.

## 2. Business Outcome

Ensure that holds, blocks, levies, garnishments, and other constraints are applied accurately and promptly when required, managed with proper priority and stacking rules, and released when conditions are met, so that the institution fulfills its legal and regulatory obligations while minimizing unnecessary disruption to customer account access.

## 3. Scope

### Included activities
- Applying holds on accounts with reason codes, amounts, effective dates, and expiration rules
- Managing hold release conditions, expiration monitoring, and manual release governance
- Processing levy and garnishment orders including intake, amount calculation, and fund segregation
- Managing constraint priority and stacking when multiple holds, levies, or blocks affect the same account
- Coordinating with payment systems to reflect hold impacts on available balances
- Publishing constraint events for downstream consumers (channels, payments, reporting)
- Maintaining constraint reason code taxonomy and permitted-action matrices
- Supporting operations dashboards for hold and constraint monitoring and exception management

### Excluded activities
- Account lifecycle state transitions (active, dormant, frozen, closed) (`cap.account_status_and_restriction_management`)
- Funds availability determination under Regulation CC for deposited items (`domain.transaction_operations`)
- Payment-level holds on individual transactions (`domain.payments`)
- Legal order case management and dispute resolution (`domain.servicing`)
- Fraud decisioning that may trigger a hold or block (`domain.risk_fraud`)
- Channel UX for displaying hold information to customers (`domain.channels_experience`)

## 4. Upstream Dependencies
- `cap.account_status_and_restriction_management` for account status context that may affect hold applicability
- `cap.account_terms_and_feature_management` for product rules governing hold behavior and limits
- `domain.servicing` for legal order intake, case management, and garnishment/levy processing instructions
- `domain.risk_fraud` for risk-driven hold and block requests
- `domain.orchestration_control` for workflow sequencing of complex constraint scenarios

## 5. Downstream Dependencies
- `domain.payments` consumes hold and constraint information to adjust available balances and transaction eligibility
- `domain.channels_experience` displays hold indicators, amounts, and expected release dates to customers and staff
- `domain.ledger_core` for segregated or earmarked funds related to levies and garnishments
- `domain.reporting_analytics` for hold volume reporting, constraint metrics, and legal order compliance dashboards
- `cap.account_status_and_restriction_management` may receive signals when constraints warrant a status change

## 6. Related Value Streams
- `vs.account_constraint_enforcement`
- `vs.regulatory_compliance_assurance`
- `vs.legal_order_fulfillment`
- `vs.deposit_account_servicing`

## 7. Related Journeys
- `journey.place_account_hold`
- `journey.release_account_hold`
- `journey.process_garnishment_levy`
- `journey.apply_regulatory_block`
- `journey.resolve_constraint_exception`

## 8. Likely Processes
- Hold application with reason coding, amount, and effective dating
- Hold release evaluation and execution (automatic expiration, manual release, condition-based release)
- Levy and garnishment order intake, validation, and fund segregation
- Constraint priority and stacking resolution when multiple constraints compete
- Hold impact calculation on available balance
- Constraint event publication for downstream consumers
- Operations monitoring for expiring, overdue, or exception holds
- Periodic review of long-standing constraints for continued validity

## 9. Risks
- Holds not applied when required by legal order, regulatory mandate, or risk decision, creating compliance exposure or financial loss
- Holds not released when eligible, causing unnecessary customer friction, complaint escalation, and potential regulatory scrutiny
- Constraint stacking conflicts where multiple holds, levies, or garnishments interact in unintended ways, producing incorrect available balances
- Levy or garnishment processing errors including incorrect amount calculation, wrong account, or missed processing deadlines
- Boundary confusion between account-level holds managed here and account status restrictions managed by `cap.account_status_and_restriction_management`

## 10. Controls
- Hold application authorization ensuring holds are applied only by authorized sources with proper documentation
- Hold expiration and release monitoring with automated alerts for holds approaching or exceeding expected duration
- Levy and garnishment compliance validation ensuring processing meets legal deadlines, amount accuracy, and jurisdictional requirements
- Constraint stacking priority rules governing how multiple concurrent constraints are ordered and resolved
- Hold audit trail capturing every hold lifecycle event with timestamp, reason, amount, authorizing source, and release conditions

## 11. Typical Systems/Providers

### Typical systems
- Hold management service
- Constraint rules engine
- Levy and garnishment processing system
- Legal order intake service
- Operations servicing workbench

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- FIS
- Thought Machine

## 12. Open Questions
- Where does the boundary sit between account-level holds governed here and payment-level holds governed in `domain.payments` when a single regulatory event affects both?
- Should levy and garnishment processing be a sub-capability given its distinct legal and compliance requirements?
- How should hold priority and stacking rules interact with Regulation CC funds availability schedules managed in `domain.transaction_operations`?
- Should constraint reason codes be shared with the restriction taxonomy in `cap.account_status_and_restriction_management` or maintained independently?
- How should this capability handle multi-jurisdictional garnishment orders with conflicting priority rules?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the management of specific encumbrances on deposit accounts distinct from lifecycle state governance. Informed by Regulation CC (Expedited Funds Availability Act) for hold context, UCC Article 4 for bank deposit and collection obligations, garnishment and levy processing requirements under federal and state law, and common core banking hold management patterns. Confidence is medium due to the inherent boundary tension between account-level holds, payment-level holds, and account status restrictions that requires further refinement as the Payments and Transaction Operations domains are modeled.
