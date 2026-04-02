---
id: cap.posting_adjustment_and_reversal_management
type: capability
name: Posting Adjustment and Reversal Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Posting Adjustment and Reversal Management

## 1. Definition

Posting Adjustment and Reversal Management is the enduring business ability to handle corrections, reversals, reclassifications, adjustments, and compensating entries to previously posted journal entries while preserving full financial lineage and audit traceability.

## 2. Business Outcome

Ensure that the institution can correct errors, reverse postings, and reclassify entries without destroying financial history, maintaining a complete and reconstructable chain of corrections that satisfies audit, regulatory, and reconciliation requirements.

## 3. Scope

### Included activities
- Processing full reversals of previously posted journal entries
- Processing partial adjustments and compensating entries
- Processing reclassification entries that move amounts between accounts or categories
- Enforcing mandatory reference linkage between corrections and original postings
- Requiring reason codes and approval for all corrections
- Enforcing prohibition on destructive overwrite of original postings
- Maintaining one-time reversal linkage to prevent duplicate reversals of the same posting
- Applying maker-checker approval workflows for sensitive or high-value corrections

### Excluded activities
- Original journal entry construction (`cap.journal_entry_construction_management`)
- Integrity validation of correction entries (handled by `cap.double_entry_integrity_control_management`)
- Posting execution of correction entries (handled by `cap.posting_execution_management`)
- Upstream event reprocessing or source system correction (belongs to originating domains)
- Reconciliation break investigation and root cause analysis (upstream process concern)

## 4. Upstream Dependencies
- Original posted entries from `cap.posting_execution_management` for reference linkage
- Correction requests from operations, reconciliation, or upstream domain processes
- Approval authority configuration for maker-checker workflows
- Reason code taxonomy for categorizing corrections

## 5. Downstream Dependencies
- `cap.journal_entry_construction_management` for constructing reversal and adjustment entries
- `cap.double_entry_integrity_control_management` for validating correction entries
- `cap.posting_execution_management` for committing correction entries to the book of record
- `cap.subledger_account_management` for updating detailed account positions after corrections
- Audit and compliance systems for correction traceability and reporting

## 6. Related Value Streams
- `vs.financial_correction`
- `vs.posting_foundation`
- `vs.accounting_control`

## 7. Related Journeys
- `journey.reverse_erroneous_posting`
- `journey.resolve_reconciliation_break`
- `journey.resolve_suspense_item`

## 8. Likely Processes
- Full reversal processing with original posting reference linkage
- Partial adjustment entry creation and processing
- Reclassification entry processing between accounts or categories
- Reason code assignment and validation
- Maker-checker approval routing for sensitive corrections
- Duplicate reversal detection and prevention
- Correction lineage recording and chain maintenance
- Correction confirmation and downstream notification

## 9. Risks
- History destruction on correction where original postings are overwritten rather than offset, making the ledger non-reconstructable
- Duplicate reversal of the same posting causing double-counted corrections
- Correction without proper authority bypassing approval requirements
- Non-reconstructable correction chain where the linkage between original and correcting entries is broken or missing
- Accumulation of unresolved suspense items due to incomplete correction workflows
- Regulatory findings due to inadequate correction audit trails

## 10. Controls
- Mandatory prior posting reference on all correction entries linking back to the original
- Correction reason code and approval requirement for every adjustment and reversal
- Prohibition on destructive overwrite ensuring original postings are never modified in place
- One-time reversal linkage preventing a posting from being reversed more than once
- Maker-checker approval for sensitive corrections exceeding defined thresholds
- Correction chain integrity validation ensuring the full history from original to correction is reconstructable

## 11. Typical Systems/Providers

### Typical systems
- Adjustment processing engines
- Reversal management services
- Correction lineage stores
- Maker-checker workflow engines

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house ledger platforms

## 12. Open Questions
- Should correction entries be processed through the same posting pipeline as original entries, or through a dedicated correction pathway with additional controls?
- How should the capability handle corrections to entries in closed accounting periods -- should period reopening be required or should corrections post to the current period with backdated effective dates?
- What thresholds should trigger maker-checker requirements for corrections (amount, account type, entry age)?
- How should reclassification entries be distinguished from reversals in downstream reporting and audit?
- Should the correction lineage store be a separate data store or integrated within the book of record?

## 13. Provenance

This capability addresses one of the most audit-sensitive areas of ledger management: the correction of financial records. Accounting standards and regulatory expectations universally require that corrections preserve the original record and create a traceable chain of adjustments. In many legacy systems, corrections are handled through ad hoc processes or destructive overwrites that compromise auditability. By extracting adjustment and reversal management as a distinct capability, the institution can enforce consistent correction governance, maintain non-destructive correction chains, and ensure that every correction is authorized, referenced, and traceable, independent of the mechanics of journal construction and posting execution.
