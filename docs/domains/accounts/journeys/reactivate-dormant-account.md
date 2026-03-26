---
id: journey.reactivate_dormant_account
type: journey
name: Reactivate Dormant Account
domain_ids:
  - domain.accounts
  - domain.customer
  - domain.identity_access
  - domain.risk_fraud
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
  - cap.account_dormancy_and_escheat_coordination
  - cap.account_status_and_restriction_management
  - cap.account_hold_and_constraint_management
  - cap.account_party_role_management
  - cap.account_maintenance_management
  - cap.customer_due_diligence_coordination
  - cap.customer_lifecycle_state_management
value_stream_id: vs.deposit_account_servicing
process_id: proc.account_reactivation_v1
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Reactivate Dormant Account

## 1. Objective

Enable a customer or institution to reactivate a deposit account that has entered a dormant or inactive state, re-establishing the account for normal transactional use after satisfying re-verification, compliance, and operational prerequisites.

## 2. Primary Actor

Retail customer requesting reactivation of a dormant account, or institutional actor initiating reactivation as part of a customer re-engagement or remediation process.

## 3. Trigger

- Customer contacts the institution to reactivate a dormant account.
- Customer attempts a transaction on a dormant account, triggering a reactivation workflow.
- Institution identifies a dormant account eligible for proactive re-engagement.

## 4. Preconditions

- The account exists and is in a dormant or inactive state (not closed or escheated).
- The requesting party can be identified and authenticated.
- Reactivation policies and re-verification requirements are available.
- The account has not progressed to an escheatment terminal state.

## 5. Happy Path

### Stage 1 — Reactivation request and eligibility assessment
The reactivation request is received and the account is evaluated for reactivation eligibility.

**Capabilities activated:** `cap.account_dormancy_and_escheat_coordination`, `cap.account_status_and_restriction_management`

The reactivation request is captured with reason and channel. The account dormancy state is confirmed. Escheatment progression is checked — if the account has progressed too far toward escheatment, reactivation may be blocked. Eligibility prerequisites are evaluated.

### Stage 2 — Customer re-verification
The institution re-verifies the customer's identity and refreshes due diligence as required by policy.

**Capabilities activated:** `cap.customer_due_diligence_coordination`

**Participating neighbor domains:** `domain.identity_access`, `domain.risk_fraud`

Identity re-verification is performed. CDD/EDD refresh is coordinated if the dormancy period exceeded policy thresholds. The Customer domain tracks completion of re-verification milestones. Neighbor domains own the actual verification and risk decisions.

### Stage 3 — Hold and constraint review
Any holds, restrictions, or constraints applied during dormancy are reviewed and resolved.

**Capabilities activated:** `cap.account_hold_and_constraint_management`, `cap.account_maintenance_management`

Dormancy-related holds or restrictions are identified. Constraints that were applied as part of dormancy processing are evaluated for removal. Any remaining valid holds (legal, regulatory) are preserved.

### Stage 4 — Party role and access re-establishment
Account-level party roles and authorizations are reviewed and re-established.

**Capabilities activated:** `cap.account_party_role_management`

**Participating neighbor domains:** `domain.identity_access`

Signatory and authorized-party roles are reviewed. Access and transactional authorities are re-enabled. Digital access credentials may need to be refreshed or re-provisioned by the Identity & Access domain.

### Stage 5 — Account status transition to active
The account is transitioned from dormant back to an active state.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.account_dormancy_and_escheat_coordination`

The account status moves from dormant to active. The reactivation date, reason, and channel are recorded. Normal transaction processing is re-enabled. The dormancy/escheatment clock is reset.

### Stage 6 — Customer lifecycle update and completion
The customer lifecycle is updated and reactivation is confirmed.

**Capabilities activated:** `cap.customer_lifecycle_state_management`

The Customer domain is notified of the reactivation. If the customer had been in a dormant or reduced-engagement lifecycle state, it is updated. Confirmation is provided to the customer.

## 6. Alternate Paths

- **Transaction-triggered reactivation** — A customer attempts a transaction on a dormant account, triggering an inline reactivation flow before the transaction can complete.
- **Institution-initiated re-engagement** — The institution proactively reaches out to dormant account holders as part of a retention or re-engagement campaign.
- **Partial reactivation with restrictions** — The account is reactivated with temporary restrictions pending completion of re-verification or due diligence refresh.
- **Reactivation of one account among several** — The customer has multiple dormant accounts and only reactivates a subset.

## 7. Failure / Exception Paths

- The account has progressed past the point of reactivation eligibility (e.g., escheatment has been initiated or completed).
- Customer identity cannot be re-verified to the required assurance level.
- Due diligence refresh reveals new risk concerns that block reactivation.
- Legal or regulatory holds prevent reactivation.
- The account product is no longer offered and reactivation requires conversion to a current product.
- The customer is deceased and reactivation is not appropriate.

## 8. Related Domains

- `domain.accounts` — owns dormancy state management, reactivation eligibility, hold resolution, status transition, and party role re-establishment
- `domain.customer` — owns customer lifecycle state updates and due diligence coordination
- `domain.identity_access` — performs identity re-verification and access re-provisioning
- `domain.risk_fraud` — owns risk re-assessment and CDD/EDD refresh decisions
- `domain.channels_experience` — owns reactivation request presentation and confirmation flows
- `domain.orchestration_control` — may coordinate multi-step reactivation workflows

## 9. Related Capabilities

- `cap.account_dormancy_and_escheat_coordination`
- `cap.account_status_and_restriction_management`
- `cap.account_hold_and_constraint_management`
- `cap.account_party_role_management`
- `cap.account_maintenance_management`
- `cap.customer_due_diligence_coordination`
- `cap.customer_lifecycle_state_management`

## 10. Value Stream Context

This journey sits inside `vs.deposit_account_servicing` as a lifecycle recovery event. It represents the re-establishment of a dormant account for active use and intersects with compliance refresh, customer retention, and escheatment prevention concerns.

## 11. Supporting Process Candidates

- `proc.account_reactivation_v1`
- `proc.dormancy_eligibility_assessment`
- `proc.customer_reverification`
- `proc.due_diligence_refresh`
- `proc.hold_and_constraint_review`
- `proc.party_role_reestablishment`
- `proc.escheatment_clock_reset`

## 12. Key Risks and Controls Encountered

### Key risks
- Reactivation of account that should have been escheated
- Identity fraud through impersonation of dormant account holder
- Reactivation without required CDD/EDD refresh
- Re-enabling access for unauthorized parties
- Reactivation of account with unresolved legal holds
- Product no longer available for the dormant account type

### Key controls
- Escheatment progression check before reactivation
- Identity re-verification at appropriate assurance level
- CDD/EDD refresh requirement based on dormancy duration
- Hold and constraint clearance review
- Party role and authorization re-validation
- Product availability check with conversion path if needed

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account management module)
- Dormancy management module
- Customer master / CIF
- Identity verification service
- KYC/evidence workbench
- Case management system (for exceptions)

### Representative providers
- Fiserv
- Jack Henry
- Temenos
- FIS
- Alloy
- LexisNexis Risk Solutions

## 14. Open Questions

- What dormancy duration thresholds should trigger mandatory CDD/EDD refresh versus simple re-verification?
- Should reactivation of a discontinued product automatically route to the Convert Account Product journey?
- How should transaction-triggered reactivation interact with real-time transaction authorization — should it block, queue, or conditionally approve?
- At what point in the escheatment progression should reactivation be permanently blocked?
- Should reactivation carry any temporary restrictions or monitoring period?

## 15. Provenance

This journey is medium-confidence because while the core reactivation flow is well understood, the intersection with escheatment progression, CDD refresh thresholds, and transaction-triggered reactivation introduces policy dependencies that vary significantly across institutions. It exercises the `cap.account_dormancy_and_escheat_coordination` capability as its primary Accounts-domain concern and demonstrates the boundary between account-level dormancy management and customer-level lifecycle state management.
