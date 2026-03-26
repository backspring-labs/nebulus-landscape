---
id: cap.account_exception_and_remediation_management
type: capability
name: Account Exception and Remediation Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Exception and Remediation Management

## 1. Definition

Account Exception and Remediation Management is the enduring business ability to detect, classify, track, and resolve account-level exceptions including processing errors, data quality defects, regulatory findings, and operational anomalies. It coordinates remediation actions across affected accounts and ensures root cause analysis, customer impact assessment, corrective action execution, remediation verification, and regulatory reporting where required.

## 2. Business Outcome

Ensure account-level exceptions are detected promptly, classified accurately, remediated completely across all affected accounts, and resolved at the root cause to prevent recurrence, while minimizing customer impact duration, satisfying regulatory remediation expectations, and maintaining a verifiable audit trail of all detection, analysis, and corrective actions.

## 3. Scope

### Included activities
- Detecting account exceptions through automated monitoring, reconciliation breaks, customer complaints, and regulatory examination findings
- Classifying exceptions by type (processing error, data quality defect, regulatory finding, operational anomaly), severity, and customer impact
- Triaging and assigning exceptions to appropriate resolution teams with defined SLAs
- Assessing customer impact scope to identify all affected accounts and quantify financial impact
- Executing remediation actions including account corrections, interest adjustments, fee reversals, and customer notifications
- Performing root cause analysis to identify the underlying process, system, or data failure
- Verifying remediation completeness through post-correction reconciliation
- Reporting remediation status to regulators, management, and affected customers as required

### Excluded activities
- Transaction-level dispute resolution and chargeback processing (`domain.disputes`)
- Fraud investigation and case management (`domain.fraud`)
- Compliance monitoring and regulatory examination management (`domain.compliance`)
- System incident management and technology problem resolution (`domain.technology`)
- Customer complaint intake and service recovery (`domain.servicing`)

## 4. Upstream Dependencies
- `cap.account_status_and_restriction_management` for account status context affecting remediation approach
- `cap.account_interest_and_fee_configuration_management` for interest and fee recalculation rules during remediation
- `domain.compliance` for regulatory findings that require account-level remediation
- `domain.servicing` for customer complaints that surface account exceptions
- `domain.transactions` for transaction processing exceptions and reconciliation breaks affecting accounts

## 5. Downstream Dependencies
- `cap.account_maintenance_management` for account attribute corrections identified during remediation
- `domain.customer` for customer notification and communication during remediation
- `domain.finance` for financial impact quantification and general ledger adjustment entries
- `domain.reporting_analytics` for exception trend analysis, remediation metrics, and management reporting
- `domain.compliance` for regulatory remediation reporting and examination response

## 6. Related Value Streams
- `vs.account_servicing_excellence`
- `vs.deposit_account_servicing`
- `vs.regulatory_compliance`
- `vs.operational_resilience`

## 7. Related Journeys
- `journey.resolve_account_processing_error`
- `journey.remediate_regulatory_finding`
- `journey.correct_account_data_defect`
- `journey.review_exception_portfolio`
- `journey.escalate_unresolved_exception`

## 8. Likely Processes
- Exception detection through automated monitoring rules and reconciliation break identification
- Exception classification, severity assessment, and SLA assignment
- Exception triage and routing to appropriate resolution team
- Customer impact assessment and affected account identification
- Remediation action planning and approval
- Remediation execution including account corrections, adjustments, and reversals
- Root cause analysis and corrective action identification
- Remediation verification through post-correction reconciliation and customer confirmation

## 9. Risks
- Undetected account exceptions persisting in production, causing ongoing customer harm, financial misstatement, or regulatory non-compliance
- Delayed remediation extending the duration of customer impact and increasing financial exposure and regulatory scrutiny
- Incomplete remediation scope where not all affected accounts are identified and corrected, leaving some customers with unresolved errors
- Remediation actions introducing new errors due to incorrect correction logic, data transformation mistakes, or inadequate testing
- Regulatory finding recurrence where root causes are not fully resolved, leading to repeat examination findings and escalating supervisory action

## 10. Controls
- Exception detection completeness monitoring validating that automated monitoring rules cover all critical account processing paths and data quality dimensions
- Remediation SLA enforcement tracking exception age against defined resolution timeframes with escalation triggers for breaches
- Remediation scope validation confirming that all affected accounts have been identified through systematic impact analysis before remediation execution begins
- Remediation pre-post reconciliation comparing account states before and after remediation to verify corrections were applied accurately and completely
- Root cause resolution verification confirming that identified root causes have been addressed through process, system, or data corrections to prevent recurrence

## 11. Typical Systems/Providers

### Typical systems
- Account exception detection engine
- Exception case management
- Remediation orchestrator
- Customer impact analyzer
- Root cause analysis workbench

### Typical providers
- Fiserv
- Jack Henry
- Appian
- Pegasystems
- ServiceNow

## 12. Open Questions
- Should account exception management own the full lifecycle from detection through root cause resolution, or should root cause resolution be delegated to the originating domain?
- How should exception management interact with enterprise incident management in `domain.technology` when exceptions have a system root cause?
- Where does the boundary sit between account-level exception management here and transaction-level dispute resolution in `domain.disputes`?
- How should mass remediation events (affecting thousands of accounts) be governed differently from individual exception resolution?
- Should exception detection rules be managed here or in the individual capabilities where exceptions originate?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on exception detection and remediation patterns for deposit accounts in US retail banking. Informed by OCC Heightened Standards for risk governance and internal controls, FDIC supervisory guidance on operational risk management, consent order remediation patterns, and common operational practices for account exception handling across core banking platforms. Confidence is high because exception detection and remediation is a well-established operational necessity with clear regulatory expectations, and every institution maintains some form of exception management for account processing errors, data quality defects, and regulatory findings.
