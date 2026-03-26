---
id: cap.account_closure_management
type: capability
name: Account Closure Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Closure Management

## 1. Definition

Account Closure Management is the enduring business ability to govern the end-to-end process of closing deposit accounts, whether initiated by the customer, the institution, or triggered by regulatory or lifecycle events such as escheatment. It encompasses closure eligibility validation, outstanding obligation resolution, final interest and fee calculation, remaining balance disbursement, downstream system notification, post-closure transaction handling, and regulatory record retention obligations triggered by account closure.

## 2. Business Outcome

Ensure every account closure is executed completely and accurately, with all outstanding obligations resolved, final balances correctly calculated and disbursed, all downstream systems properly notified, and all regulatory record retention and disclosure requirements satisfied, while minimizing customer friction for voluntary closures and protecting the institution from risk exposure on involuntary closures.

## 3. Scope

### Included activities
- Validating closure eligibility by checking for outstanding obligations, pending transactions, holds, liens, and regulatory restrictions
- Resolving outstanding obligations that block closure (pending ACH, outstanding checks, overdraft balances, fee accruals)
- Calculating final interest accrued, prorated fees, and early closure penalties where applicable
- Disbursing remaining account balance via customer-selected method (check, transfer, wire)
- Notifying all downstream systems, channels, and service providers of account closure
- Handling post-closure transaction arrivals (ACH returns, check presentments, recurring debits)
- Triggering record retention schedules for closed account records
- Generating closure confirmation notices and regulatory disclosures

### Excluded activities
- Account dormancy detection and escheat coordination (`cap.account_dormancy_and_escheat_coordination`)
- Account status transitions other than closure (`cap.account_status_and_restriction_management`)
- Customer offboarding and relationship closure when all accounts are closed (`domain.customer`)
- Dispute resolution for transactions occurring before closure (`domain.disputes`)
- Payment instrument revocation (card cancellation, check stop-payment) as standalone activities (`domain.payments`)

## 4. Upstream Dependencies
- `cap.account_status_and_restriction_management` for current account status, restrictions, and holds that may block closure
- `cap.account_hold_and_constraint_management` for active holds and liens that must be resolved before closure
- `cap.account_interest_and_fee_configuration_management` for interest accrual and fee calculation rules at closure
- `cap.account_overdraft_and_protection_management` for outstanding overdraft balances requiring resolution
- `domain.transactions` for pending transaction status and post-closure transaction handling rules
- `domain.identity_access` for authenticated and authorized closure request validation

## 5. Downstream Dependencies
- `cap.account_record_retention_and_archival` for retention schedule activation on closed account records
- `domain.payments` for payment instrument revocation, recurring payment cancellation, and ACH return processing
- `domain.channels_experience` for closure confirmation display and post-closure account access removal
- `domain.finance` for general ledger entries reflecting account closure, balance disbursement, and fee recognition
- `domain.reporting_analytics` for closure volume reporting, closure reason analysis, and retention metrics

## 6. Related Value Streams
- `vs.account_lifecycle_management`
- `vs.deposit_account_servicing`
- `vs.regulatory_compliance`
- `vs.customer_offboarding`

## 7. Related Journeys
- `journey.close_account_customer_initiated`
- `journey.close_account_bank_initiated`
- `journey.resolve_closure_blockers`
- `journey.disburse_closing_balance`

## 8. Likely Processes
- Closure request intake and authorization verification
- Closure eligibility validation and blocker identification
- Outstanding obligation resolution and pending transaction management
- Final interest calculation, fee proration, and early closure penalty assessment
- Closing balance calculation and disbursement execution
- Downstream system closure notification and confirmation
- Post-closure transaction handling (returns, rejects, redirects)
- Closure record retention schedule activation and confirmation generation

## 9. Risks
- Premature closure with outstanding obligations (pending ACH, uncollected deposits, outstanding checks) resulting in financial loss or customer harm
- Closing balance disbursement errors including incorrect amounts, wrong disbursement method, or funds sent to an outdated or unauthorized destination
- Unauthorized account closure initiated without proper authentication or authorization, potentially through social engineering
- Closure record retention gaps where required records are not properly retained or are prematurely destroyed, creating regulatory exposure
- Post-closure transaction arrivals (ACH debits, check presentments) that are not properly handled, resulting in return item fees, customer confusion, or financial loss

## 10. Controls
- Closure eligibility checklist enforcement requiring systematic verification of all closure prerequisites before execution
- Closing balance reconciliation validating that the disbursed amount equals the account balance after all final interest, fees, and adjustments
- Closure authorization verification confirming the requester's identity, authority, and relationship to the account
- Post-closure transaction handling rules defining automated processing for transactions arriving after closure (return ACH, reject checks, notify originators)
- Closure record retention compliance confirming retention schedules are activated and records are preserved per regulatory requirements

## 11. Typical Systems/Providers

### Typical systems
- Account closure orchestrator
- Closing balance calculator
- Closure disbursement service
- Closure notification publisher
- Post-closure transaction handler

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Q2
- Finastra

## 12. Open Questions
- Should account closure management own the resolution of all closure blockers, or should it delegate to the originating capability (e.g., holds to `cap.account_hold_and_constraint_management`)?
- How should partial closure be handled for accounts that are part of a sweep or relationship structure?
- Where does the boundary sit between account-level closure managed here and customer-relationship-level offboarding managed by `domain.customer`?
- How should closure interact with accounts that have linked overdraft protection or other cross-account dependencies?
- Should post-closure transaction handling rules be owned here or by `domain.payments`?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the account closure lifecycle for deposit accounts in US retail banking. Informed by Regulation DD (Truth in Savings) for final disclosure obligations, Regulation E for EFT-related closure considerations, Regulation CC for check hold implications at closure, BSA/AML record retention requirements, and common operational patterns for closure processing across core banking platforms. Confidence is high because account closure is a well-defined, regulatory-driven lifecycle event with established processing patterns across all US depository institutions.
