---
id: proc.sar_filing_v1
type: process
name: SAR Filing v1
journey_id: journey.file_suspicious_activity_report
capability_ids:
  - cap.suspicious_activity_reporting
  - cap.case_investigation_management
  - cap.alert_triage_and_disposition
  - cap.regulatory_information_sharing_and_law_enforcement_response
  - cap.financial_crime_program_governance_and_testing
  - cap.compliance_issue_and_remediation_management
status: draft
source_refs:
  - src.fincen_sar_faq_2025
  - src.ffiec_bsaaml_manual_2026
  - src.ffiec_suspicious_activity_monitoring_appendix_s
last_reviewed: 2026-03-27
confidence: high
---

# SAR Filing v1

## 1. Objective

Operationally execute the controlled sequence required to prepare, review, approve, and electronically file a Suspicious Activity Report (SAR) with FinCEN, ensuring that reportable suspicious activity is documented with a clear, complete, and useful narrative, filed within regulatory deadlines, and supported by a defensible evidence trail for continuing activity monitoring, law enforcement response, and regulatory examination.

## 2. Scope

This process covers:
- case referral evaluation and SAR filing determination
- SAR narrative and FinCEN form preparation
- quality review and BSA officer or compliance approval
- electronic filing submission and confirmation
- post-filing record management and retention
- continuing activity review and supplemental/continuing SAR scheduling
- law enforcement referral and 314(a)/314(b) information sharing

This process does **not** own:
- underlying investigation and case disposition (covered by `proc.fraud_alert_investigation_v1` or other investigation processes)
- transaction monitoring rule execution (`cap.transaction_monitoring`)
- customer relationship actions (account closure, restriction) resulting from SAR filing (`domain.accounts`, `domain.customer`)
- payment blocking or reversal (`domain.payments`)

## 3. Trigger

A case investigation disposition determines that suspicious activity has been identified meeting SAR filing thresholds, a mandatory filing trigger is activated (e.g., insider abuse, computer intrusion), or a continuing activity review determines that additional SAR filing is required.

## 4. Entry Conditions

- A completed or substantially complete investigation case exists with documented findings.
- The suspicious activity meets regulatory filing thresholds ($5,000 with known subject; $25,000 regardless of subject identification for most institutions).
- The SAR filing system and FinCEN BSA E-Filing gateway are operational.
- BSA officer or designated approver is available for review and approval.
- Subject and transaction data are accessible for form population.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.sar_filing.case_referral_and_filing_determination` | Case Referral and Filing Determination | `domain.risk_fraud` |
| `stage.sar_filing.sar_narrative_and_form_preparation` | SAR Narrative and Form Preparation | `domain.risk_fraud` |
| `stage.sar_filing.quality_review_and_compliance_approval` | Quality Review and Compliance Approval | `domain.risk_fraud` |
| `stage.sar_filing.electronic_filing_and_confirmation` | Electronic Filing and Confirmation | `domain.risk_fraud` |
| `stage.sar_filing.post_filing_record_management` | Post-Filing Record Management | `domain.risk_fraud` |
| `stage.sar_filing.continuing_activity_review` | Continuing Activity Review | `domain.risk_fraud` |
| `stage.sar_filing.law_enforcement_referral_and_information_sharing` | Law Enforcement Referral and Information Sharing | `domain.risk_fraud` |

## 6. Stage-by-Stage Flow

### Stage: `stage.sar_filing.case_referral_and_filing_determination`

**Description:** A completed investigation case is referred to the SAR filing team or BSA unit for filing determination. The referral is evaluated to confirm that the activity meets SAR filing thresholds and that a SAR is the appropriate regulatory response. The filing determination considers the type of suspicious activity, dollar thresholds, subject identification status, and whether a prior SAR exists for the same subject or activity.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.case_investigation_management`

**Systems involved:** `sys.case_management_system`, `sys.sar_filing_system`, `sys.customer_master`, `sys.core_banking_platform`

**Decision points:**
- Does the activity meet the applicable dollar threshold for SAR filing?
- Is the subject identified, partially identified, or unknown?
- Is this a mandatory filing category (insider abuse, computer intrusion, BSA structuring)?
- Is this a new SAR, a continuing SAR, or a corrected/amended SAR?
- Has a prior SAR been filed for the same subject or related activity?

**Risks:** `risk.missed_sar_filing_obligation`, `risk.threshold_miscalculation`, `risk.duplicate_filing`, `risk.incorrect_filing_category`

**Controls:** `ctrl.sar_filing_trigger_rules`, `ctrl.threshold_verification_checklist`, `ctrl.prior_sar_search_requirement`, `ctrl.filing_category_determination_standards`

**Evidence generated:** Filing determination record, threshold analysis, prior SAR search results, filing category assignment, referral acceptance or rejection with rationale

**Handoff:** If filing determination is positive, move to SAR narrative and form preparation. If the referral does not meet filing criteria, document rationale and return to case management.

### Stage: `stage.sar_filing.sar_narrative_and_form_preparation`

**Description:** The SAR analyst or BSA specialist prepares the FinCEN SAR form and drafts the narrative section. The narrative must describe the suspicious activity clearly and completely, answering who, what, when, where, why, and how. Subject information, financial institution information, and suspicious activity characterization fields are populated from case and customer data. The narrative is the most examination-sensitive component of the SAR.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.case_investigation_management`

**Systems involved:** `sys.sar_filing_system`, `sys.case_management_system`, `sys.customer_master`, `sys.core_banking_platform`, `sys.transaction_monitoring_system`

**Decision points:**
- Is all required subject information available and accurately populated?
- Does the narrative adequately describe the suspicious activity (who, what, when, where, why, how)?
- Are all relevant transactions and dollar amounts included?
- Should additional suspicious activity characterization types be checked?
- Is the filing institution information correct?

**Risks:** `risk.sar_narrative_deficiency`, `risk.incomplete_subject_information`, `risk.inaccurate_transaction_data`, `risk.narrative_contains_prohibited_content`

**Controls:** `ctrl.sar_narrative_quality_checklist`, `ctrl.subject_information_completeness_gate`, `ctrl.transaction_data_reconciliation`, `ctrl.prohibited_content_review` (e.g., no mention of SAR existence to subject)

**Evidence generated:** Draft SAR form, narrative text, subject information compilation, transaction summary supporting the SAR, characterization type selections

**Handoff:** Draft SAR prepared; move to quality review and compliance approval.

### Stage: `stage.sar_filing.quality_review_and_compliance_approval`

**Description:** The draft SAR undergoes quality review by a senior analyst or QA function, followed by formal approval by the BSA officer or designated compliance approver. The review evaluates narrative completeness and clarity, form accuracy, supporting evidence alignment, and regulatory compliance. Deficiencies are returned for remediation before approval.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.financial_crime_program_governance_and_testing`

**Systems involved:** `sys.sar_filing_system`, `sys.case_management_system`

**Decision points:**
- Does the narrative meet quality standards for clarity, completeness, and usefulness?
- Is the form accurately populated with no missing required fields?
- Does the SAR align with the underlying investigation findings and evidence?
- Are there any compliance issues requiring remediation before filing?
- Does the BSA officer approve the SAR for filing?

**Risks:** `risk.quality_review_bottleneck`, `risk.filing_deadline_breach`, `risk.approval_without_adequate_review`, `risk.narrative_inconsistency_with_evidence`

**Controls:** `ctrl.sar_narrative_quality_checklist`, `ctrl.filing_deadline_tracking`, `ctrl.bsa_officer_approval_requirement`, `ctrl.qa_remediation_tracking`

**Evidence generated:** QA review record with findings, remediation records if applicable, BSA officer approval record with timestamp, filing deadline compliance status

**Handoff:** SAR approved by BSA officer; move to electronic filing.

### Stage: `stage.sar_filing.electronic_filing_and_confirmation`

**Description:** The approved SAR is submitted electronically to FinCEN through the BSA E-Filing system. The filing confirmation (including the BSA ID/tracking number) is received and recorded. Filing must occur within 30 calendar days of the date the suspicious activity was initially detected (with a 60-day extension if the subject is not identified at initial detection).

**Capabilities activated:** `cap.suspicious_activity_reporting`

**Systems involved:** `sys.sar_filing_system`, `sys.fincen_bsa_efiling_gateway`

**Decision points:**
- Is the filing within the regulatory deadline?
- Was the electronic submission accepted or rejected by FinCEN?
- If rejected, what remediation is required for resubmission?

**Risks:** `risk.filing_deadline_breach`, `risk.efiling_submission_failure`, `risk.filing_rejection_by_fincen`

**Controls:** `ctrl.filing_deadline_tracking`, `ctrl.submission_confirmation_verification`, `ctrl.rejection_remediation_sla`

**Evidence generated:** Filing submission record, FinCEN acceptance/rejection response, BSA ID/tracking number, filing timestamp, deadline compliance record

**Handoff:** Filing confirmed; move to post-filing record management.

### Stage: `stage.sar_filing.post_filing_record_management`

**Description:** Following successful filing, the SAR and all supporting documentation are archived in accordance with BSA record retention requirements (5 years from date of filing). The SAR record is linked to the originating case, and the filing is indexed for future search, continuing activity review, and regulatory examination access.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.compliance_issue_and_remediation_management`

**Systems involved:** `sys.sar_filing_system`, `sys.case_management_system`, `sys.evidence_repository`, `sys.records_management_system`

**Decision points:**
- Are all required supporting documents archived with the SAR record?
- Is the record retention period correctly set (minimum 5 years)?
- Is the SAR record properly indexed for search and examination access?
- Should any customer-facing or account-level actions be triggered post-filing?

**Risks:** `risk.inadequate_record_retention`, `risk.disconnected_sar_and_case_records`, `risk.premature_record_destruction`

**Controls:** `ctrl.sar_record_retention_policy`, `ctrl.record_completeness_verification`, `ctrl.retention_period_enforcement`, `ctrl.sar_case_linkage_validation`

**Evidence generated:** Archive confirmation, retention period assignment, case-to-SAR linkage record, examination access index entry

**Handoff:** Records archived; evaluate for continuing activity review and law enforcement referral.

### Stage: `stage.sar_filing.continuing_activity_review`

**Description:** For subjects with filed SARs, continuing activity is monitored to determine whether additional SAR filings are required. Continuing SARs must be filed at least every 90 days if the suspicious activity persists. The review evaluates whether the activity has continued, changed, or ceased, and determines the appropriate filing response.

**Capabilities activated:** `cap.suspicious_activity_reporting`, `cap.case_investigation_management`, `cap.alert_triage_and_disposition`

**Systems involved:** `sys.sar_filing_system`, `sys.case_management_system`, `sys.transaction_monitoring_system`, `sys.customer_master`

**Decision points:**
- Has the suspicious activity continued since the prior SAR filing?
- Has the nature or scope of the activity changed materially?
- Is a continuing SAR required within the 90-day window?
- Should the investigation case be reopened or expanded?
- Has the activity ceased, allowing the continuing review cycle to end?

**Risks:** `risk.continuing_activity_oversight_gap`, `risk.continuing_sar_deadline_missed`, `risk.stale_monitoring_without_fresh_analysis`

**Controls:** `ctrl.continuing_activity_review_schedule`, `ctrl.continuing_sar_deadline_tracking`, `ctrl.activity_change_assessment_standards`

**Evidence generated:** Continuing activity review record, activity status determination, continuing SAR filing or cessation decision, updated investigation notes if applicable

**Handoff:** If continuing SAR required, cycle back to SAR narrative and form preparation. If activity has ceased, document and close the continuing review cycle.

### Stage: `stage.sar_filing.law_enforcement_referral_and_information_sharing`

**Description:** When warranted by the severity or nature of the suspicious activity, the process coordinates law enforcement referral and participation in information-sharing programs. This includes voluntary law enforcement notifications, responses to FinCEN 314(a) requests, and participation in 314(b) voluntary information sharing with other financial institutions. Anti-tipping provisions are strictly observed throughout.

**Capabilities activated:** `cap.regulatory_information_sharing_and_law_enforcement_response`, `cap.suspicious_activity_reporting`

**Systems involved:** `sys.case_management_system`, `sys.sar_filing_system`, `sys.fincen_bsa_efiling_gateway`, `sys.evidence_repository`

**Decision points:**
- Does the activity warrant voluntary law enforcement referral beyond the SAR filing?
- Has a 314(a) request been received that matches this subject?
- Is 314(b) information sharing with other institutions appropriate?
- What information can be shared without violating anti-tipping provisions?

**Risks:** `risk.premature_customer_tipping`, `risk.inappropriate_information_disclosure`, `risk.missed_314a_response_deadline`, `risk.inadequate_law_enforcement_coordination`

**Controls:** `ctrl.anti_tipping_controls`, `ctrl.314a_response_deadline_tracking`, `ctrl.314b_sharing_authorization`, `ctrl.law_enforcement_referral_documentation`

**Evidence generated:** Law enforcement referral record, 314(a) response records, 314(b) information sharing records, anti-tipping compliance documentation

**Handoff:** Referral and sharing activities completed and documented. Process returns to continuing activity review cycle or closes if no further action is required.

## 7. Decision Points

Key decisions across the process:
- Filing determination: meets threshold vs does not meet threshold
- Filing category: new vs continuing vs amended
- Narrative quality: adequate vs requires remediation
- BSA officer approval: approved vs returned for revision
- Filing submission: accepted vs rejected by FinCEN
- Continuing activity: persists vs ceased
- Law enforcement referral: warranted vs not warranted

## 8. Alternate Outcomes

- Referral evaluated and determined not to meet SAR filing thresholds; documented and returned
- SAR filed as initial report with continuing activity review scheduled
- Continuing SAR filed at 90-day interval
- SAR filing triggers voluntary law enforcement notification
- 314(a) match identified during SAR filing process
- SAR rejected by FinCEN for technical errors; remediated and refiled
- Activity ceases; continuing review cycle terminated with documentation

## 9. Risks (process-level)

- Missed SAR filing obligation due to referral process gaps
- Narrative deficiency undermining SAR usefulness to law enforcement
- Filing deadline breaches due to quality review bottlenecks
- Incomplete subject information reducing SAR effectiveness
- Customer tipping through inadvertent disclosure of SAR existence
- Continuing activity monitoring gaps allowing undetected ongoing activity
- Inadequate record retention failing examination requirements

## 10. Controls (process-level)

- SAR filing trigger rules and threshold verification
- Narrative quality checklist and QA review standards
- Filing deadline tracking with escalation triggers
- Subject information completeness gate
- Anti-tipping controls across all customer-facing touchpoints
- Continuing activity review schedule and deadline enforcement
- BSA officer approval requirement for all filings
- Record retention policy enforcement (minimum 5 years)

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- filing determination records with threshold analysis
- SAR form drafts and final versions
- narrative quality review records
- BSA officer approval records with timestamps
- FinCEN submission and acceptance/rejection records
- BSA ID/tracking numbers
- post-filing archive records with retention periods
- continuing activity review records and decisions
- law enforcement referral and information sharing records
- 314(a) response records and 314(b) sharing records
- anti-tipping compliance documentation

## 12. Systems Involved (summary)

- case management system
- SAR filing system
- FinCEN BSA E-Filing gateway
- customer master / CIF
- core banking platform
- transaction monitoring system
- evidence repository
- records management system

## 13. Provider Participation

Representative providers often adjacent to this process:
- Verafin, NICE Actimize, Oracle Financial Services, SAS, Hue (formerly Unit21), Abrigo (formerly Banker's Toolbox), Alessa (formerly CaseWare RCM), Actimize SAR Manager, FinCEN BSA E-Filing

## 14. Open Questions

- Should the SAR quality review stage be split into peer review and BSA officer approval as distinct sub-stages?
- How should the process handle SARs that span multiple case investigations or subjects?
- Should the continuing activity review process be modeled as a separate recurring process artifact?
- How should the process address SAR filing for activity involving the institution's own employees (insider SARs)?

## 15. Provenance

This process is the primary regulatory reporting obligation within `domain.risk_fraud` and carries direct examination and enforcement consequences. FinCEN and federal banking examiners evaluate SAR filing timeliness, narrative quality, completeness, and the institution's overall suspicious activity reporting program as a core component of BSA/AML program effectiveness assessment. Deficiencies in SAR filing are among the most common findings in BSA examination and enforcement actions.
