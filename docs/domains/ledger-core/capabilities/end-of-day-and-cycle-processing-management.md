---
id: cap.end_of_day_and_cycle_processing_management
type: capability
name: End of Day and Cycle Processing Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# End of Day and Cycle Processing Management

## 1. Definition

End of Day and Cycle Processing Management is the enduring business ability to execute accounting cycle events such as day close, statement-cycle close, accrual roll-forward, fee runs, dormancy treatment hooks, and posting window transitions in the correct sequence and with governed controls over rerun and reopen conditions.

## 2. Business Outcome

Ensure that all accounting cycle events execute in the correct order, exactly once per cycle, with full evidence capture, so that balances, accruals, fees, and period boundaries are advanced reliably and the institution can produce accurate financial positions at any point in the processing calendar.

## 3. Scope

### Included activities
- Executing end-of-day close processing including balance finalization and cutoff enforcement
- Managing statement cycle close events and statement period boundaries
- Executing accrual roll-forward processing for interest and other accrual-based items
- Triggering scheduled fee assessment and posting runs as part of cycle processing
- Managing posting window open and close transitions
- Enforcing cycle step sequencing and preventing out-of-order execution
- Capturing cycle step evidence for audit and operational traceability
- Governing rerun and reopen conditions when cycle steps must be repeated

### Excluded activities
- Interest calculation logic (`cap.interest_accrual_management`)
- Fee assessment logic (`cap.fee_assessment_management`)
- Period close and financial reporting rollups (`cap.period_close_support_management`)
- Statement generation and delivery (channel domain)
- Dormancy determination logic (`cap.account_dormancy_and_escheat_coordination`)

## 4. Upstream Dependencies
- Posting completion status from `cap.posting_execution_management` to confirm all entries are finalized before close
- Accrual computation results from `cap.interest_accrual_management` for roll-forward processing
- Fee assessment results from `cap.fee_assessment_management` for fee run execution
- Account status information from `cap.account_status_and_restriction_management` for dormancy treatment hooks

## 5. Downstream Dependencies
- `cap.balance_computation_management` for balance finalization at day close
- `cap.period_close_support_management` for higher-level period close orchestration
- `cap.financial_audit_trail_and_lineage_management` for cycle evidence preservation
- Statement generation processes consuming cycle close outputs
- Regulatory and management reporting relying on cycle-closed balances

## 6. Related Value Streams
- `vs.accounting_cycle_operations`
- `vs.financial_truth_creation`
- `vs.period_close_support`

## 7. Related Journeys
- `journey.process_end_of_day_close`
- `journey.execute_interest_accrual_cycle`
- `journey.execute_monthly_fee_assessment`

## 8. Likely Processes
- Day close execution including posting cutoff, balance finalization, and close confirmation
- Statement cycle close execution and statement period boundary advancement
- Accrual roll-forward processing advancing accrued amounts to the next period
- Fee run execution triggering scheduled fee assessment and posting cycles
- Posting window open and close transition management
- Cycle step sequencing enforcement and dependency validation
- Cycle evidence capture and retention for each executed step
- Rerun and reopen governance for cycle steps that must be repeated

## 9. Risks
- Cycle logic executing out of sequence, causing dependent steps to operate on incomplete data
- Duplicate cycle step execution where the same step runs more than once without governance
- Accrual roll-forward omission where accrued amounts are not advanced, distorting period-end balances
- Posting window closing with unresolved conditions such as pending entries or exception items
- Cycle step failure without detection, leaving the processing calendar in an inconsistent state
- Rerun or reopen without proper governance, creating duplicate financial effects

## 10. Controls
- Cycle sequencing controls enforcing correct step order and prerequisite completion before each step begins
- One-time cycle execution controls preventing duplicate step execution within the same cycle
- Open/close state governance ensuring posting windows transition cleanly with no unresolved items
- Rerun and reopen controls requiring explicit authorization and evidence of the conditions necessitating repetition
- Cycle step evidence capture preserving execution timestamps, inputs, outputs, and completion status for every step
- Cycle failure detection and alerting ensuring incomplete steps are identified and escalated before downstream processing continues

## 11. Typical Systems/Providers

### Typical systems
- Cycle processing engines and orchestrators
- Day close services
- Posting window managers
- Cycle evidence stores
- Batch scheduling and monitoring platforms

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- FIS
- In-house batch processing platforms

## 12. Open Questions
- Should cycle processing support partial close where some account segments close while others remain open?
- How should the system handle cycle processing for accounts that span multiple time zones?
- What is the correct behavior when a cycle step fails midway through execution -- should the entire step be rolled back or should partial progress be preserved?
- Should dormancy treatment hooks be executed as part of day close or as a separate, independently scheduled cycle?
- How should cycle processing handle real-time posting systems where there is no traditional posting window?

## 13. Provenance

This capability reflects the operational reality that banking ledgers operate on defined processing cycles -- daily, monthly, quarterly -- and that the correct sequencing and governance of cycle events is essential to financial accuracy. End-of-day processing is not merely a batch job; it is the mechanism by which the institution advances its financial position, finalizes balances, rolls forward accruals, and establishes the boundaries between accounting periods. Failures in cycle processing -- out-of-sequence execution, duplicate runs, missed steps -- can produce material financial errors that are difficult to detect and costly to correct. Extracting cycle processing management as a distinct capability allows the institution to govern the integrity of its processing calendar independently from the individual computations (interest, fees, balances) that execute within each cycle.
