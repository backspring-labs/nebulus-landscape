---
id: cap.account_record_retention_and_archival
type: capability
name: Account Record Retention and Archival
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Record Retention and Archival

## 1. Definition

Account Record Retention and Archival is the enduring business ability to govern the retention, archival, retrieval, and disposition of account records throughout and beyond the account lifecycle. It ensures that account-related records are retained for the required duration under applicable federal and state regulations, archived in a manner that preserves their integrity and retrievability, protected from premature destruction by litigation holds and regulatory examination orders, and disposed of systematically when all retention obligations have been satisfied.

## 2. Business Outcome

Ensure the institution can produce accurate, complete, and timely account records in response to regulatory examinations, legal discovery requests, customer inquiries, and dispute resolution needs, while systematically disposing of records that have satisfied all retention obligations to minimize storage costs and reduce data exposure risk.

## 3. Scope

### Included activities
- Applying retention schedules to account records based on record type, regulatory requirements, and account lifecycle stage
- Archiving account records from active systems to long-term storage while preserving data integrity and retrievability
- Retrieving archived account records in response to authorized requests (regulatory examination, legal discovery, customer inquiry)
- Managing litigation holds that override normal retention and disposition schedules
- Executing disposition of account records that have satisfied all applicable retention periods and are not subject to any hold
- Monitoring retention compliance across the account record portfolio
- Coordinating retention requirements across multiple regulatory regimes (BSA/AML, Regulation E, IRS, state requirements)

### Excluded activities
- Account transaction record creation and initial storage (`domain.transactions`)
- Enterprise-wide records management policy definition (`domain.compliance`, `domain.legal`)
- Document imaging and digitization of physical account records (`domain.operations`)
- Data privacy and right-to-erasure requests that conflict with retention obligations (`domain.compliance`)
- Email and communication retention not specific to account records (`domain.technology`)

## 4. Upstream Dependencies
- `cap.account_closure_management` for closure events that trigger retention schedule activation on closed account records
- `cap.account_opening` for account origination records subject to retention requirements
- `domain.transactions` for transaction records associated with account history
- `domain.legal` for litigation hold directives and legal discovery requests
- `domain.compliance` for regulatory retention schedule definitions and examination record requests

## 5. Downstream Dependencies
- `domain.compliance` consumes retention compliance reports and produces records for regulatory examinations
- `domain.legal` consumes archived records for legal discovery and litigation support
- `domain.reporting_analytics` for retention portfolio metrics, disposition volume reporting, and compliance dashboards
- `domain.customer` for customer-facing record retrieval in response to account history requests
- `domain.audit` for retention compliance audit evidence and disposition audit trails

## 6. Related Value Streams
- `vs.regulatory_compliance`
- `vs.account_lifecycle_management`
- `vs.operational_resilience`
- `vs.legal_and_compliance_readiness`

## 7. Related Journeys
- `journey.retrieve_archived_account_record`
- `journey.respond_to_legal_discovery_request`
- `journey.execute_retention_disposition`
- `journey.apply_litigation_hold`

## 8. Likely Processes
- Retention schedule assignment based on record type and applicable regulatory requirements
- Account record archival execution with integrity verification
- Archived record retrieval in response to authorized requests
- Litigation hold application and release management
- Retention disposition eligibility assessment and approval
- Disposition execution with audit trail and confirmation
- Retention compliance monitoring and exception reporting
- Cross-regulation retention period reconciliation for records subject to multiple requirements

## 9. Risks
- Premature record destruction where account records are disposed of before all applicable retention periods have expired, creating regulatory exposure and inability to respond to examinations or litigation
- Retention schedule non-compliance where records are not classified or scheduled correctly, resulting in inconsistent retention across similar record types
- Archived record irretrievability where archived records cannot be located, accessed, or read due to storage degradation, format obsolescence, or indexing errors
- Litigation hold breach where records subject to a legal hold are inadvertently destroyed through automated disposition processes
- Excessive retention beyond required periods increasing storage costs, data breach exposure, and the volume of records potentially discoverable in litigation

## 10. Controls
- Retention schedule enforcement ensuring all account records are classified and assigned a retention period before archival
- Disposition approval workflow requiring documented verification that all retention obligations and holds are satisfied before record destruction
- Litigation hold override prevention ensuring automated disposition processes cannot destroy records subject to an active legal or regulatory hold
- Archival integrity verification confirming that archived records remain intact, readable, and retrievable through periodic sampling and validation
- Retention compliance audit providing periodic assessment of retention schedule adherence, disposition completeness, and hold management effectiveness

## 11. Typical Systems/Providers

### Typical systems
- Record retention policy engine
- Account record archive
- Archived record retrieval service
- Litigation hold manager
- Retention disposition orchestrator

### Typical providers
- Hyland
- OpenText
- Iron Mountain
- Veritas
- IBM

## 12. Open Questions
- Should account record retention be managed as an account-domain capability or as part of an enterprise-wide records management capability in `domain.compliance`?
- How should retention schedules handle records that span multiple regulatory regimes with different retention periods (e.g., a transaction record subject to both BSA 5-year and Reg E 2-year requirements)?
- Where does the boundary sit between account-level record retention managed here and transaction-level record retention managed by `domain.transactions`?
- How should retention and archival handle records in legacy systems that cannot be migrated to the enterprise archive?
- Should disposition be fully automated based on schedule expiry, or should it always require human approval?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the record retention and archival obligations for deposit account records in US retail banking. Informed by BSA/AML record-keeping requirements (31 CFR 1020), Regulation E dispute window implications (12 CFR 1005), IRS reporting and record retention obligations, state-specific record retention statutes, and Federal Rules of Civil Procedure for litigation hold requirements. Confidence is high because record retention is a well-established regulatory obligation with clear requirements across multiple federal and state regulatory regimes applicable to all US depository institutions.
