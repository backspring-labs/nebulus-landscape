---
id: cap.subledger_account_management
type: capability
name: Subledger Account Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Subledger Account Management

## 1. Definition

Subledger Account Management is the enduring business ability to maintain customer-facing and product-facing financial subledgers, including posting positions, transaction lineage, and the relationship between detailed subledger accounts and institution-level general ledger books.

## 2. Business Outcome

Ensure that the institution maintains accurate, detailed financial records at the customer and product level that reconcile to the general ledger, enabling accurate balance reporting, customer-facing transaction history, regulatory reporting, and break resolution at the appropriate level of detail.

## 3. Scope

### Included activities
- Creating and configuring subledger accounts with appropriate GL mapping
- Maintaining posting positions (running balances) at the subledger account level
- Recording transaction-level posting history beneath each subledger account
- Performing subledger-to-GL rollup aggregation for reconciliation
- Enforcing controlled subledger creation with proper account assignment rules
- Maintaining full posting history retention for audit and regulatory purposes
- Supporting balance inquiries and position lookups at the subledger level

### Excluded activities
- General ledger chart-of-accounts maintenance and GL structure management
- Journal entry construction and posting execution (`cap.journal_entry_construction_management`, `cap.posting_execution_management`)
- Account opening and product origination (`domain.accounts`)
- Customer balance presentation and statement rendering (downstream channel capabilities)
- Interest calculation and fee computation (upstream product capabilities)
- Reconciliation break detection and investigation (reconciliation domain capabilities)

## 4. Upstream Dependencies
- Posted journal entries from `cap.posting_execution_management` for position updates
- Account metadata from `domain.accounts` for subledger account setup and context
- Product configuration for determining subledger structure and GL mapping
- Chart-of-accounts definitions for GL rollup targets

## 5. Downstream Dependencies
- Balance derivation and inquiry capabilities for serving customer-facing balance requests
- Statement and transaction history capabilities for rendering subledger activity
- Reconciliation capabilities for comparing subledger positions against GL totals
- Regulatory reporting for detailed transaction-level evidence
- `cap.posting_adjustment_and_reversal_management` for correction impact on subledger positions

## 6. Related Value Streams
- `vs.detailed_financial_accountability`
- `vs.posting_foundation`
- `vs.balance_derivation`

## 7. Related Journeys
- `journey.post_deposit_transaction`
- `journey.compute_customer_balance`
- `journey.resolve_reconciliation_break`

## 8. Likely Processes
- Subledger account creation and GL mapping assignment
- Posting position update after journal entry commitment
- Transaction lineage recording at the subledger level
- Subledger-to-GL rollup computation and reconciliation
- Subledger balance inquiry and position lookup
- Posting history retention and archival management
- Subledger account closure and final position verification

## 9. Risks
- Subledger-to-GL mismatch where detailed subledger positions do not reconcile to general ledger totals
- Posting to the wrong detailed account causing customer-visible balance errors
- Orphaned subledger history where transaction records become disconnected from their account context
- Broken transaction lineage making it impossible to reconstruct the sequence of postings to an account
- Subledger position drift due to missed or out-of-order posting updates
- Data volume growth in posting history stores causing performance degradation in balance queries

## 10. Controls
- Controlled subledger creation and mapping ensuring accounts are only created with valid GL assignments
- Posting account assignment rules preventing postings to invalid or closed subledger accounts
- Rollup reconciliation controls periodically comparing subledger totals to GL balances and alerting on breaks
- Full posting history retention ensuring no transaction records are purged before regulatory retention periods expire
- Subledger position integrity checks validating that running balances equal the sum of all posted entries
- Account closure controls requiring zero balance and no pending postings before subledger deactivation

## 11. Typical Systems/Providers

### Typical systems
- Subledger engines and detailed account stores
- Transaction lineage services and posting history databases
- GL rollup services and reconciliation engines
- Balance inquiry and position lookup services

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- FIS
- In-house ledger platforms

## 12. Open Questions
- Should subledger position maintenance be synchronous (updated inline with posting) or asynchronous (derived from posted entry streams)?
- How should the subledger handle multi-currency accounts where positions must be maintained in both transaction and reporting currencies?
- What is the correct retention period for detailed posting history, and should archival be handled within the subledger engine or by a separate data lifecycle capability?
- How should subledger-to-GL rollup reconciliation handle timing differences when postings and rollups are processed asynchronously?
- Should the subledger support virtual or synthetic accounts for internal management accounting purposes alongside customer-facing accounts?

## 13. Provenance

This capability reflects the fundamental accounting need to maintain detailed financial records at a granularity below the general ledger. In banking, subledgers serve as the bridge between individual customer and product transactions and the institution-level financial statements. While the general ledger provides aggregate truth, the subledger provides the detailed evidence and transaction lineage that supports customer inquiries, dispute resolution, regulatory examination, and reconciliation. Extracting subledger management as a distinct capability allows the institution to independently govern the accuracy, completeness, and retention of detailed financial records, separate from GL management, posting execution, and balance presentation concerns.
