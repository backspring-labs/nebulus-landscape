---
id: journey.close_account
type: journey
name: Close Account
domain_ids:
  - domain.accounts
  - domain.customer
  - domain.payments
  - domain.ledger_core
  - domain.risk_fraud
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
  - cap.account_closure_management
  - cap.account_status_and_restriction_management
  - cap.account_hold_and_constraint_management
  - cap.account_interest_and_fee_configuration_management
  - cap.account_record_retention_and_archival
  - cap.account_party_role_management
  - cap.account_exception_and_remediation_management
  - cap.customer_lifecycle_state_management
value_stream_id: vs.deposit_account_servicing
process_id: proc.account_closure_v1
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Close Account

## 1. Objective

Enable a customer or institution to close a deposit account through a controlled path that resolves all outstanding balances and obligations, clears holds and restrictions, disburses remaining funds, satisfies regulatory retention requirements, and transitions the account to a terminal closed state.

## 2. Primary Actor

Retail customer requesting account closure, or institutional actor initiating closure for cause (e.g., risk, inactivity, escheatment progression).

## 3. Trigger

- Customer requests account closure through a digital channel, branch, or call center.
- Institution initiates closure due to risk decision, prolonged dormancy, regulatory action, or escheatment.

## 4. Preconditions

- The account exists and is in a state eligible for closure (active, dormant, or restricted — not already closed or in a terminal state).
- The requesting party has authority to close the account.
- Closure policies and retention rules are available.

## 5. Happy Path

### Stage 1 — Closure request and eligibility assessment
The closure request is received and the account is evaluated for closure eligibility.

**Capabilities activated:** `cap.account_closure_management`, `cap.account_status_and_restriction_management`

The closure request is captured with reason and channel. The account status is checked for closure eligibility. Outstanding obligations, pending transactions, and blocking conditions are identified.

### Stage 2 — Hold and constraint resolution
All holds, liens, and constraints on the account are reviewed and resolved or cleared.

**Capabilities activated:** `cap.account_hold_and_constraint_management`, `cap.account_exception_and_remediation_management`

Active holds are identified and resolved. Legal or regulatory constraints are evaluated. Exceptions requiring remediation are surfaced and handled before closure can proceed.

### Stage 3 — Interest accrual and fee finalization
Accrued interest is calculated through the closure date and any final fees are assessed.

**Capabilities activated:** `cap.account_interest_and_fee_configuration_management`

**Participating neighbor domains:** `domain.ledger_core`

Final interest is accrued. Closing fees or early-termination charges are assessed per product terms. The account balance is finalized.

### Stage 4 — Balance disbursement
The remaining balance is disbursed to the customer through an approved method.

**Capabilities activated:** `cap.account_closure_management`

**Participating neighbor domains:** `domain.payments`

Disbursement method is determined (transfer, check, cash). Remaining funds are moved. Zero-balance confirmation is recorded.

### Stage 5 — Party role deactivation
Account-level party roles and authorizations are deactivated.

**Capabilities activated:** `cap.account_party_role_management`

Signatory and authorized-party roles are deactivated. Access and transactional authorities linked to this account are revoked.

### Stage 6 — Account status transition to closed
The account is transitioned to a terminal closed state.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.account_closure_management`

The account status moves to closed. The closure reason, date, and channel are recorded. No further transactions are permitted.

### Stage 7 — Retention, archival, and customer lifecycle update
Retention rules are applied and the customer lifecycle is updated if this was the last active account.

**Capabilities activated:** `cap.account_record_retention_and_archival`, `cap.customer_lifecycle_state_management`

Record retention policies are applied based on product type and regulatory requirements. Archival is initiated. If the customer no longer holds any active accounts, the Customer domain is notified to evaluate lifecycle state changes.

## 6. Alternate Paths

- **Institution-initiated closure** — The institution initiates closure for cause (risk, fraud, dormancy, escheatment). The customer may not be the requesting actor. Additional controls and notification requirements apply.
- **Partial closure of joint account** — One party requests closure but other parties wish to retain the account, triggering a party role change rather than full closure.
- **Closure with pending items** — Outstanding items (e.g., pending ACH, uncollected checks) must settle before closure can complete, resulting in a closure-pending state.
- **Closure with negative balance** — The account has an outstanding obligation that must be collected or written off before reaching terminal state.

## 7. Failure / Exception Paths

- Active legal holds or regulatory restrictions prevent closure.
- Pending transactions cannot be resolved within the closure window.
- Disbursement method fails or is rejected.
- The requesting party lacks authority to close the account.
- Negative balance cannot be collected and requires charge-off coordination.
- Retention or archival policies cannot be determined for the product type.

## 8. Related Domains

- `domain.accounts` — owns closure eligibility, hold resolution, status transition, retention, and party role deactivation
- `domain.customer` — owns customer lifecycle state updates when the last account is closed
- `domain.payments` — owns balance disbursement and fund movement
- `domain.ledger_core` — owns final interest accrual and balance reconciliation
- `domain.risk_fraud` — may initiate closure for cause; owns risk-related closure decisions
- `domain.channels_experience` — owns closure request presentation and confirmation flows
- `domain.orchestration_control` — may coordinate multi-step closure workflows

## 9. Related Capabilities

- `cap.account_closure_management`
- `cap.account_status_and_restriction_management`
- `cap.account_hold_and_constraint_management`
- `cap.account_interest_and_fee_configuration_management`
- `cap.account_record_retention_and_archival`
- `cap.account_party_role_management`
- `cap.account_exception_and_remediation_management`
- `cap.customer_lifecycle_state_management`

## 10. Value Stream Context

This journey sits inside `vs.deposit_account_servicing` as the terminal lifecycle event. It represents the controlled wind-down of an account contract and intersects with retention, compliance, and customer relationship continuity concerns.

## 11. Supporting Process Candidates

- `proc.account_closure_v1`
- `proc.closure_eligibility_assessment`
- `proc.hold_and_constraint_resolution`
- `proc.final_interest_and_fee_calculation`
- `proc.balance_disbursement`
- `proc.party_role_deactivation`
- `proc.record_retention_initiation`

## 12. Key Risks and Controls Encountered

### Key risks
- Closure of account with unresolved legal holds or liens
- Disbursement to incorrect party or method
- Premature closure before pending transactions settle
- Loss of required records due to incomplete retention
- Unauthorized closure request
- Failure to update customer lifecycle when last account closes

### Key controls
- Hold and constraint clearance gate before closure
- Authority verification for closure requestor
- Pending transaction settlement check
- Disbursement confirmation and zero-balance validation
- Retention policy application validation
- Customer lifecycle state evaluation trigger on last-account closure

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account management module)
- Payment/disbursement service
- Hold and lien management system
- Record retention and archival platform
- Customer master / CIF
- Case management system (for exception handling)

### Representative providers
- Fiserv
- Jack Henry
- Temenos
- FIS
- Iron Mountain (archival)

## 14. Open Questions

- Should closure with a negative balance be modeled as a distinct journey or a variant path within this journey?
- How should partial closure of joint accounts be handled — as a closure variant or a separate party-role-change journey?
- What is the boundary between Accounts-domain retention responsibilities and enterprise-level data governance?
- Should institution-initiated closure for escheatment be a separate journey given its distinct regulatory requirements?

## 15. Provenance

This journey represents the terminal lifecycle event for deposit accounts. It exercises key Accounts-domain capabilities around closure management, hold resolution, retention, and party role deactivation. It is paired with the Open Checking Account and Open Savings Account journeys to form a complete lifecycle arc from opening through closure.
