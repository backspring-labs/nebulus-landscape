---
id: journey.file_suspicious_activity_report
type: journey
name: File Suspicious Activity Report
domain_ids:
  - domain.risk_fraud
  - domain.customer
  - domain.accounts
  - domain.payments
capability_ids:
  - cap.suspicious_activity_reporting
  - cap.case_investigation_management
  - cap.alert_triage_and_disposition
  - cap.regulatory_information_sharing_and_law_enforcement_response
  - cap.financial_crime_program_governance_and_testing
  - cap.compliance_issue_and_remediation_management
value_stream_id: vs.financial_crime_prevention
process_id: proc.sar_filing_v1
status: draft
source_refs:
  - src.fincen_sar_faq_2025
  - src.ffiec_bsaaml_manual_2026
  - src.ffiec_suspicious_activity_monitoring_appendix_s
last_reviewed: 2026-03-27
confidence: high
---

# File Suspicious Activity Report

## 1. Objective

Prepare, review, approve, and electronically file a Suspicious Activity Report (SAR) with FinCEN when the institution identifies activity that is known or suspected to involve funds derived from illegal activity, is designed to evade BSA requirements, lacks a lawful purpose, or involves the use of the institution to facilitate criminal activity -- all within the regulatory-mandated 30-day filing window.

## 2. Primary Actor

A BSA officer or authorized senior analyst within the institution's BSA/AML compliance team who holds filing authority and is responsible for the accuracy and completeness of the SAR submission.

## 3. Trigger

A case investigation reaches a disposition that the underlying activity meets the SAR filing threshold as defined by BSA regulations and institutional policy. This may result from fraud investigation, transaction monitoring alert disposition, sanctions screening escalation, or referral from a business unit.

## 4. Preconditions

- A completed investigation case exists with documented findings supporting the filing decision.
- The BSA officer or authorized analyst has access to the case record, supporting evidence, and all relevant customer, account, and transaction data.
- The institution's SAR filing system is connected to FinCEN's BSA E-Filing System and credentials are current.
- SAR narrative standards, templates, and quality checklists are published and available.
- Filing authority delegation and approval workflows are configured in the case management system.

## 5. Happy Path

### Stage 1 -- Filing decision and case referral
The investigating analyst or case manager determines that the investigated activity meets the SAR filing threshold. The case is referred to the SAR preparation queue with a recommended filing rationale and supporting documentation.

**Capabilities activated:** `cap.case_investigation_management`, `cap.alert_triage_and_disposition`

### Stage 2 -- SAR preparation and narrative drafting
The BSA analyst or SAR preparer assembles the required SAR fields: subject information, suspicious activity characterization, instrument types, amounts, and date ranges. The SAR narrative is drafted following FinCEN guidance, describing the who, what, when, where, why, and how of the suspicious activity in clear, factual language.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.case_investigation_management`

### Stage 3 -- Quality review and approval
The draft SAR undergoes quality review against institutional standards and FinCEN filing requirements. The reviewer checks completeness, accuracy, narrative clarity, and consistency with the underlying case evidence. Deficiencies are returned for correction. Upon passing review, the BSA officer or authorized delegate approves the SAR for filing.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.financial_crime_program_governance_and_testing`

### Stage 4 -- Electronic filing with FinCEN
The approved SAR is submitted electronically through FinCEN's BSA E-Filing System. The system returns a confirmation and a BSA Identifier (BSA ID) that is recorded in the institution's case management system as proof of filing.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.regulatory_information_sharing_and_law_enforcement_response`

### Stage 5 -- Post-filing actions and continuing review
The SAR filing is linked to the originating case. Continuing activity monitoring is established for the subject. If the suspicious activity continues, a continuing SAR is scheduled within 90 days. The case record is updated with filing confirmation, and applicable metrics are recorded.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.case_investigation_management`, `cap.compliance_issue_and_remediation_management`

## 6. Alternate Paths

- Continuing SAR is filed for ongoing suspicious activity by the same subject, referencing the original SAR and documenting new activity.
- Joint SAR is filed in coordination with another financial institution involved in the same suspicious activity.
- SAR is filed on a subject who is no longer a customer of the institution (historical activity or exited relationship).
- Voluntary SAR is filed for activity below the mandatory threshold but deemed suspicious by the institution.
- Law enforcement contact occurs before SAR filing, and the SAR narrative references the law enforcement interaction without compromising the investigation.

## 7. Failure / Exception Paths

- SAR preparation exceeds the 30-day filing deadline, triggering a compliance exception and requiring late-filing documentation.
- BSA E-Filing System is unavailable, requiring retry procedures and deadline extension documentation.
- Quality review identifies material deficiencies that cannot be resolved within the filing window.
- SAR is filed with errors and requires an amended or corrected filing.
- Subject of the SAR is tipped off about the filing, violating the SAR confidentiality requirement (31 USC 5318(g)(2)).
- Filing authority is unavailable (illness, departure) and delegation procedures must be invoked.

## 8. Related Domains

- `domain.risk_fraud` -- owns the SAR filing lifecycle, investigation case management, and regulatory reporting
- `domain.customer` -- provides subject identity, relationship history, and CDD/EDD data for the SAR narrative
- `domain.accounts` -- provides account details, transaction history, and activity patterns referenced in the SAR
- `domain.payments` -- provides payment instrument and transaction details for suspicious transactions described in the SAR

## 9. Related Capabilities

- `cap.suspicious_activity_reporting`
- `cap.case_investigation_management`
- `cap.alert_triage_and_disposition`
- `cap.regulatory_information_sharing_and_law_enforcement_response`
- `cap.financial_crime_program_governance_and_testing`
- `cap.compliance_issue_and_remediation_management`

## 10. Value Stream Context

This journey is a critical regulatory output of `vs.financial_crime_prevention`. SAR filing is a legal obligation under the Bank Secrecy Act, and filing failures carry significant regulatory penalties, enforcement actions, and reputational risk. The quality and timeliness of SAR filings is a primary metric evaluated during BSA/AML examinations and is directly linked to the institution's regulatory standing.

## 11. Supporting Process Candidates

- `proc.sar_filing_v1`
- `proc.sar_narrative_drafting`
- `proc.sar_quality_review`
- `proc.sar_approval_workflow`
- `proc.bsa_efiling_submission`
- `proc.continuing_sar_scheduling`
- `proc.sar_confidentiality_controls`
- `proc.filing_deadline_monitoring`

## 12. Key Risks and Controls Encountered

### Key risks
- Late SAR filing violates the 30-day regulatory mandate
- SAR narrative is incomplete, inaccurate, or insufficiently descriptive for law enforcement use
- SAR confidentiality is breached through tipping off the subject
- Filing system connectivity failure prevents timely submission
- Inadequate quality review allows deficient SARs to be filed
- Continuing activity is not identified, and follow-up SARs are not filed
- Filing authority gaps create bottlenecks or unauthorized filings

### Key controls
- Filing deadline tracking with automated escalation alerts
- Mandatory quality review checklist aligned with FinCEN guidance
- SAR confidentiality training and access restrictions (need-to-know basis)
- BSA E-Filing connectivity monitoring and retry procedures
- Dual-authorization for SAR approval (preparer and approver separation)
- Continuing activity monitoring triggers linked to filed SARs
- Comprehensive audit trail for all SAR preparation, review, and filing actions
- Periodic SAR quality backtesting by compliance or internal audit

## 13. Typical Systems and Providers Involved

### Typical systems
- Case management and investigation platform
- SAR preparation and narrative management tool
- FinCEN BSA E-Filing System (government-hosted)
- Customer and account data repositories
- Transaction history and payment detail systems
- Document management and evidence repository
- Filing deadline and workflow tracking system

### Representative providers
- Verafin (Nasdaq)
- NICE Actimize SAR Manager
- Oracle Financial Services (OFSS)
- SAS Anti-Money Laundering
- Pega Financial Crime
- Hummingbird (acquired by Abrigo)
- Abrigo SAR Filing

## 14. Open Questions

- How should the institution leverage structured SAR data fields versus narrative content to maximize law enforcement utility?
- What role should AI-assisted narrative drafting play in SAR preparation, and how should it be governed?
- How should the institution track and measure the quality of SAR narratives beyond regulatory compliance minimums?
- Should continuing SAR review periods be risk-adjusted based on the severity or type of suspicious activity?
- How should cross-border suspicious activity be reported when multiple jurisdictions and reporting obligations apply?

## 15. Provenance

This journey represents the institution's primary regulatory reporting obligation under the Bank Secrecy Act. SAR filing is the culmination of the detection, monitoring, and investigation lifecycle. Grounded in FinCEN SAR FAQ (2025), FFIEC BSA/AML Examination Manual (2026), and FFIEC Appendix S on suspicious activity monitoring. The 30-day filing requirement, SAR confidentiality provisions, and continuing SAR obligations are codified in 31 CFR 1020.320 and 31 USC 5318(g).
