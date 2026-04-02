---
id: cap.general_ledger_interface_management
type: capability
name: General Ledger Interface Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# General Ledger Interface Management

## 1. Definition

General Ledger Interface Management is the enduring business ability to maintain the controlled relationship between banking subledgers and enterprise general ledger structures, including summarization rules, classification mapping, and posting handoff orchestration.

## 2. Business Outcome

Ensure that subledger-level financial activity is accurately summarized, correctly classified, and reliably handed off to the enterprise general ledger, enabling the institution to produce accurate financial statements, satisfy regulatory reporting requirements, and maintain an unbroken lineage from detailed transactions to aggregate GL positions.

## 3. Scope

### Included activities
- Maintaining mapping tables that relate subledger accounts and posting categories to GL chart-of-accounts entries
- Performing subledger-to-GL rollup summarization at configurable aggregation levels
- Executing batch handoff of summarized postings to the enterprise GL system
- Validating control totals between subledger detail and GL summary during handoff
- Managing GL classification changes when chart-of-accounts structures evolve
- Reporting on unmapped or unclassified items that cannot be handed off
- Maintaining detail-to-summary lineage for audit and reconciliation

### Excluded activities
- Subledger account creation and position maintenance (`cap.subledger_account_management`)
- Journal entry construction and posting execution (`cap.journal_entry_construction_management`, `cap.posting_execution_management`)
- Enterprise GL chart-of-accounts maintenance (finance/ERP domain responsibility)
- Financial statement preparation and regulatory report generation (downstream finance capabilities)
- Reconciliation break investigation and resolution (reconciliation domain capabilities)

## 4. Upstream Dependencies
- Posted subledger positions from `cap.subledger_account_management` and `cap.posting_execution_management`
- Chart-of-accounts definitions from the enterprise GL or ERP system
- Product and account configuration for determining classification mapping rules
- Period close signals indicating when handoff batches should be generated

## 5. Downstream Dependencies
- Enterprise GL systems that receive summarized posting batches
- Financial reporting and regulatory reporting capabilities that depend on GL accuracy
- Reconciliation capabilities that compare subledger totals against GL positions
- Audit capabilities that trace GL entries back to subledger detail
- Period close orchestration that depends on successful GL handoff completion

## 6. Related Value Streams
- `vs.finance_alignment`
- `vs.accounting_control`
- `vs.period_close_support`

## 7. Related Journeys
- `journey.process_end_of_day_close`
- `journey.close_financial_period`
- `journey.resolve_reconciliation_break`

## 8. Likely Processes
- Subledger-to-GL rollup summarization at scheduled intervals
- GL mapping table maintenance and version control
- GL handoff batch assembly and transmission
- Control total validation between subledger detail and GL summary
- Unmapped item exception identification and reporting
- GL classification remapping when chart-of-accounts changes occur
- Handoff confirmation and acknowledgment processing

## 9. Risks
- Subledger-to-GL mismatch where summarized totals do not reconcile to detailed subledger positions
- Incorrect finance classification mapping causing postings to land in the wrong GL accounts
- Untraceable rollup lineage making it impossible to explain GL balances from subledger detail
- Incomplete GL handoff where some subledger activity is omitted from the batch
- Stale mapping tables that do not reflect current chart-of-accounts structures
- Timing mismatches between subledger posting cutoff and GL handoff batch generation

## 10. Controls
- Governed mapping tables with version control and change approval workflows
- Rollup validation and control totals comparing subledger detail sums to GL summary amounts before handoff
- Detail-to-summary lineage ensuring every GL entry can be traced back to its constituent subledger postings
- Unmapped item exception reporting that surfaces items with no valid GL classification before handoff
- Handoff batch completeness checks ensuring all expected subledger accounts are represented
- Mapping change impact analysis before deploying chart-of-accounts updates

## 11. Typical Systems/Providers

### Typical systems
- GL interface engines and handoff orchestrators
- Rollup mapping services and classification registries
- GL handoff batch processors
- Finance classification registries and mapping stores

### Typical providers
- Temenos
- Thought Machine
- Fiserv
- SAP
- In-house GL integration platforms

## 12. Open Questions
- Should GL handoff be real-time (streaming) or batch-oriented, and what are the trade-offs for period close timing?
- How should the interface handle multi-entity or multi-currency GL structures where a single subledger feeds multiple GL books?
- What governance process should apply when chart-of-accounts changes require remapping of historical subledger classifications?
- How should unmapped items be handled during period close when resolution cannot occur before the close deadline?
- Should the GL interface support retroactive reclassification of previously handed-off entries?

## 13. Provenance

This capability reflects the fundamental need in banking to bridge the gap between detailed operational subledgers and the enterprise general ledger. Banking systems typically maintain transaction-level detail in product-specific subledgers while the enterprise GL operates at a summarized, classification-oriented level suitable for financial reporting. The interface between these layers requires careful mapping governance, control total validation, and lineage preservation to ensure that institutional financial statements accurately reflect the underlying business activity. Extracting GL interface management as a distinct capability allows the institution to independently govern the accuracy and completeness of the subledger-to-GL relationship, separate from subledger maintenance, posting execution, and GL administration concerns.
