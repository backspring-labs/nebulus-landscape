---
id: cap.period_close_support_management
type: capability
name: Period Close Support Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Period Close Support Management

## 1. Definition

Period Close Support Management is the enduring business ability to support financial close, rollups, and control evidence assembly needed for reporting periods, finance operations, and regulatory production, ensuring that the ledger's contribution to close is complete, governed, and delivered on schedule.

## 2. Business Outcome

Ensure that the institution can close financial periods with confidence that all ledger-sourced balances, rollups, and control evidence are complete and accurate, that unresolved conditions are escalated before close, and that post-close corrections are governed to prevent ungoverned changes to reported figures.

## 3. Scope

### Included activities
- Assessing close readiness by validating that all prerequisite conditions are met before close proceeds
- Assembling close rollups aggregating subledger and GL balances for reporting period boundaries
- Packaging close evidence including reconciliation results, suspense reviews, and control attestations
- Governing post-close reopen and rerun conditions when corrections must be applied after close
- Escalating unresolved items that could affect close quality
- Coordinating delivery of ledger close outputs to finance and regulatory reporting consumers
- Maintaining close checklists and signoff workflows

### Excluded activities
- Day-level cycle processing and posting window management (`cap.end_of_day_and_cycle_processing_management`)
- Reconciliation execution (`cap.financial_reconciliation_management`)
- Suspense resolution (`cap.suspense_and_exception_account_management`)
- Financial statement preparation and disclosure (finance domain)
- Regulatory return preparation and submission (regulatory reporting domain)
- External audit coordination (audit domain)

## 4. Upstream Dependencies
- Cycle-closed balances from `cap.end_of_day_and_cycle_processing_management` confirming all daily cycles are complete
- Reconciliation results and evidence from `cap.financial_reconciliation_management`
- Suspense position assessments from `cap.suspense_and_exception_account_management`
- Lineage completeness assessments from `cap.financial_audit_trail_and_lineage_management`
- Subledger and GL balances from `cap.subledger_account_management` and `cap.general_ledger_interface_management`

## 5. Downstream Dependencies
- Finance operations consuming close rollups for financial statement preparation
- Regulatory reporting processes consuming close outputs for regulatory return production
- External audit processes consuming close evidence packages
- Management reporting relying on period-closed figures
- Post-close governance processes managing any corrections applied after close

## 6. Related Value Streams
- `vs.period_close_support`
- `vs.financial_governance`
- `vs.regulatory_reporting_readiness`

## 7. Related Journeys
- `journey.close_financial_period`
- `journey.process_end_of_day_close`

## 8. Likely Processes
- Close readiness assessment validating that all prerequisite conditions -- reconciliation completion, suspense review, lineage checks -- are satisfied
- Close rollup assembly aggregating balances and producing period-boundary figures for reporting consumers
- Close evidence packaging assembling reconciliation results, control attestations, and lineage completeness reports
- Post-close reopen governance controlling when and how corrections can be applied after a period is closed
- Unresolved item escalation at close surfacing items that could affect close quality for management decision
- Close signoff and delivery coordinating authorized approval and transmission of close outputs to consumers
- Close checklist management tracking completion of all close activities across contributing teams

## 9. Risks
- Close proceeds with unresolved ledger risks where material reconciliation breaks, suspense items, or lineage gaps are not addressed
- Incomplete close rollups where not all required subledger and GL populations are included in period-boundary aggregations
- Post-close corrections applied without governance where changes to closed-period figures are made without proper authorization
- Regulatory production unsupported where close outputs do not contain the data or evidence needed for regulatory return preparation
- Close timeline failures where prerequisite activities are not completed in time for close deadlines
- Evidence gaps where close packages are missing required attestations or supporting documentation

## 10. Controls
- Close readiness criteria defining the prerequisite conditions that must be satisfied before close can proceed
- Evidence completeness checks validating that all required reconciliation, suspense, and lineage evidence is assembled
- Unresolved item escalation at close requiring explicit management decision on any outstanding items before close is approved
- Reopen and rerun governance requiring authorized approval, documented justification, and impact assessment before any closed period is reopened
- Close signoff and delivery controls requiring authorized approval before close outputs are transmitted to downstream consumers
- Close checklist enforcement tracking all required activities and blocking close until all items are complete

## 11. Typical Systems/Providers

### Typical systems
- Close support services and orchestration platforms
- Rollup assembly engines
- Close evidence repositories
- Close readiness dashboards
- Close workflow and checklist management tools

### Typical providers
- Temenos
- Fiserv
- SAP
- FIS
- In-house close management platforms

## 12. Open Questions
- Should close support be a centralized service that orchestrates all contributing domains, or should each domain manage its own close contribution independently?
- How should the system handle soft close versus hard close, where preliminary figures are produced before final close is approved?
- What is the correct governance for post-close adjustments that affect regulatory returns already submitted?
- Should close readiness assessment be automated with configurable criteria, or should it remain a judgment-based process?
- How should close support handle multi-entity consolidation where different legal entities have different close calendars?

## 13. Provenance

This capability reflects the banking requirement that financial periods must be closed in a governed manner, with confidence that all contributing ledger data is complete, reconciled, and supported by evidence. Period close is not merely an accounting formality; it is the process by which the institution certifies the accuracy of the figures it will report to regulators, investors, and management. Failures in close -- proceeding with unresolved breaks, missing evidence, or ungoverned post-close corrections -- can result in restatements, regulatory findings, and loss of confidence in financial reporting. Extracting period close support as a distinct capability allows the institution to govern close readiness, evidence assembly, and post-close governance independently from the individual capabilities that produce the underlying data.
