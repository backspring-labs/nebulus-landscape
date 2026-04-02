---
id: cap.memo_and_pending_financial_position_management
type: capability
name: Memo and Pending Financial Position Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Memo and Pending Financial Position Management

## 1. Definition

Memo and Pending Financial Position Management is the enduring business ability to maintain memo, pending, and pre-posted financial states that influence spendability or visibility before final accounting recognition, ensuring clear distinction between posted truth and provisional positions.

## 2. Business Outcome

Ensure that the institution accurately reflects the customer's financial position including items that have not yet been formally posted, enabling correct available balance computation, preventing overspending against uncommitted funds, and maintaining a clear audit trail of the lifecycle from pending state to final posting.

## 3. Scope

### Included activities
- Creating and maintaining memo positions that reflect anticipated or in-flight financial impacts
- Managing pending item lifecycles from creation through resolution (posting, expiry, or cancellation)
- Maintaining holds that reduce spendability without affecting posted balances
- Converting pending positions to posted status upon settlement or clearing
- Monitoring aged pending items and enforcing expiry rules
- Preserving linkage between pending items and their eventual posted entries
- Distinguishing posted from pending states in all position representations

### Excluded activities
- Journal entry construction and posting execution (`cap.posting_execution_management`)
- Balance computation and derivation (`cap.balance_computation_management`)
- Payment authorization and fraud screening (`domain.payments`, `domain.financial_crime`)
- Account hold placement for legal or regulatory reasons (account domain hold management)
- Customer notification of pending item status changes (channel domain)

## 4. Upstream Dependencies
- Transaction initiation events from `domain.payments` and other originating domains
- Authorization responses that create initial memo positions
- Account configuration from `domain.accounts` for hold and pending item rules
- Clearing and settlement signals that trigger pending-to-posted conversion

## 5. Downstream Dependencies
- `cap.balance_computation_management` for incorporating pending positions into available balance calculations
- Customer-facing balance and transaction displays that show pending items
- `cap.posting_execution_management` when pending items convert to posted entries
- Reconciliation capabilities that track pending item resolution
- Dispute and exception management capabilities that reference pending item history

## 6. Related Value Streams
- `vs.financial_position_management`
- `vs.balance_derivation`
- `vs.customer_financial_visibility`

## 7. Related Journeys
- `journey.compute_customer_balance`
- `journey.post_deposit_transaction`

## 8. Likely Processes
- Memo position creation upon transaction authorization or initiation
- Pending position lifecycle management through intermediate states
- Hold placement and amount adjustment based on authorization updates
- Pending-to-posted conversion upon clearing or settlement confirmation
- Hold expiry and automatic release based on configured time limits
- Aged pending item monitoring and exception escalation
- Pending item cancellation and position reversal

## 9. Risks
- Pending items treated as posted, causing incorrect ledger balances or premature financial recognition
- Stale holds never released, permanently reducing customer spendability without justification
- Spendability distortion where available balance does not accurately reflect the customer's true financial position
- Pending-to-posted linkage break where the connection between a memo item and its final posted entry is lost
- Duplicate pending positions created for the same underlying transaction
- Timing issues where pending items are visible to customers but not yet reflected in balance computations

## 10. Controls
- Explicit posted-versus-pending state distinction enforced at the data model level, preventing conflation
- Hold expiry and release governance ensuring holds are automatically released after configured time limits
- Pending-to-posting linkage requiring every posted entry that originated as a pending item to reference its source
- Aged pending item monitoring with alerts and escalation for items exceeding expected lifecycle durations
- Duplicate pending item detection preventing multiple memo positions for the same transaction
- Pending position integrity checks ensuring the sum of pending items reconciles to expected totals

## 11. Typical Systems/Providers

### Typical systems
- Memo position services and authorization hold engines
- Pending position stores and lifecycle managers
- Hold management engines with expiry and release logic
- Position conversion services bridging pending to posted states

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house real-time position engines

## 12. Open Questions
- Should memo positions be maintained in the same store as posted positions, or in a separate pre-posting layer?
- How should the system handle partial clearing where only a portion of a pending item converts to posted status?
- What is the appropriate default hold expiry period, and should it vary by transaction type or channel?
- How should pending positions interact with overdraft protection -- should pending items trigger protection evaluation or only posted items?
- Should pending item history be retained after conversion to posted status, and for how long?

## 13. Provenance

This capability reflects the reality that in modern banking, a customer's financial position is influenced by items that have not yet achieved final accounting recognition. Authorizations, pending deposits, in-flight transfers, and clearing-stage items all affect what the customer can spend, even though they have not been formally posted to the ledger. Managing these provisional states as a distinct capability allows the institution to govern the accuracy of available balance computation, enforce clear boundaries between posted truth and pending positions, and maintain the lifecycle traceability that supports dispute resolution, reconciliation, and regulatory examination.
