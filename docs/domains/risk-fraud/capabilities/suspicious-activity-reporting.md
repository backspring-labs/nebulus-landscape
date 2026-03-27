---
id: cap.suspicious_activity_reporting
type: capability
name: Suspicious Activity Reporting
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Suspicious Activity Reporting

## 1. Definition

Suspicious Activity Reporting is the enduring business ability to prepare, review, approve, file, and manage Suspicious Activity Reports (SARs) and equivalent regulatory filings (such as STRs in non-US jurisdictions) with applicable authorities in response to identified suspicious activity, ensuring timely, accurate, and complete reporting in compliance with BSA/AML and other financial crime regulations, while maintaining strict confidentiality and supporting continuing activity monitoring and refiling obligations.

## 2. Business Outcome

Ensure the institution fulfills its legal obligation to report suspicious activity to regulatory authorities within mandated timeframes, files accurate and complete SARs that provide useful intelligence to law enforcement, maintains strict confidentiality of SAR existence and content, identifies and refiles on continuing suspicious activity, and demonstrates a well-governed and regulatory-defensible reporting process that withstands examination and audit scrutiny.

## 3. Scope

### Included activities
- Identifying SAR filing triggers from case investigation determinations and other sources
- Drafting SAR narratives that accurately and completely describe the suspicious activity, subjects, and supporting facts
- Assembling supporting transaction data, subject information, and activity summaries for SAR content
- Reviewing SAR drafts for accuracy, completeness, narrative quality, and regulatory compliance
- Approving SARs through appropriate authority levels before filing
- Electronically filing SARs with FinCEN (or equivalent authority) via BSA E-Filing or designated systems
- Monitoring for continuing suspicious activity that triggers refiling obligations
- Preparing and filing SAR amendments and corrections
- Maintaining SAR records and confidentiality in compliance with retention requirements
- Responding to regulatory inquiries about filed SARs

### Excluded activities
- Investigation and case determination that precedes SAR filing decisions (`cap.case_investigation_management`)
- Alert generation and detection (`cap.transaction_monitoring`, `cap.fraud_detection_and_scoring`)
- Currency Transaction Report (CTR) filing and other non-SAR regulatory reporting
- Customer notification or communication regarding SAR filings (prohibited by law)
- Law enforcement coordination beyond SAR filing (regulatory information sharing capabilities)
- Account restriction or closure decisions related to SAR subjects (`cap.risk_restriction_and_offboarding_decisioning`)

## 4. Upstream Dependencies
- Case investigation determinations and findings from `cap.case_investigation_management`
- Subject information, customer profiles, and identification data from `domain.customer`
- Transaction data and activity summaries from `domain.payments` / `domain.transactions`
- Account information from `domain.accounts`
- Prior SAR filing history for subjects and related activity
- Risk rating and due diligence information from `cap.customer_and_entity_risk_rating` and `cap.enhanced_due_diligence_and_periodic_review_decisioning`

## 5. Downstream Dependencies
- FinCEN BSA E-Filing system (or equivalent regulatory authority filing gateway) for SAR submission
- Regulatory information sharing and law enforcement response capabilities for post-filing coordination
- `cap.risk_restriction_and_offboarding_decisioning` for SAR-informed account action decisions
- `cap.customer_and_entity_risk_rating` for SAR-informed risk rating updates
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for SAR-triggered enhanced due diligence
- Continuing activity monitoring for refiling trigger identification
- Management and board reporting for SAR filing volumes, timeliness, and program metrics

## 6. Related Value Streams
- `vs.regulatory_compliance`
- `vs.financial_crime_detection`
- `vs.risk_management`

## 7. Related Journeys
- `journey.sar_drafting_and_narrative_preparation`
- `journey.sar_review_and_approval`
- `journey.sar_filing_and_submission`
- `journey.sar_amendment_and_continuing_activity`
- `journey.sar_regulatory_inquiry_response`

## 8. Likely Processes
- SAR filing trigger identification and initiation from case determinations
- SAR narrative drafting and supporting data assembly
- SAR quality review for accuracy, completeness, and narrative standards
- SAR approval workflow through designated authority levels
- SAR electronic filing and submission to FinCEN or equivalent authority
- SAR filing confirmation and acknowledgment tracking
- Continuing activity monitoring and refiling trigger identification
- SAR amendment and correction preparation and filing
- SAR recordkeeping, retention, and confidentiality management
- SAR filing timeliness monitoring and deadline tracking
- SAR volume, quality, and timeliness reporting and metrics
- SAR regulatory inquiry response and document production

## 9. Risks
- Late or missed SAR filing violates regulatory requirements and creates significant enforcement exposure
- Inaccurate or incomplete SAR narratives fail to convey the nature and significance of suspicious activity to law enforcement
- SAR filings not meeting regulatory content and format standards are rejected or fail to provide useful intelligence
- Unauthorized disclosure of SAR existence or content violates federal law and compromises investigations
- Failure to identify continuing activity for refiling results in incomplete regulatory reporting obligations
- Inconsistent SAR filing thresholds and standards across investigation teams create regulatory risk
- Insufficient SAR quality review results in filings that do not withstand regulatory examination scrutiny
- Loss of SAR records or inability to produce filing history creates compliance and examination challenges

## 10. Controls
- SAR filing deadline monitoring with escalation for approaching regulatory timeframes
- SAR narrative quality review against defined standards and regulatory expectations
- SAR completeness validation for all required fields and content elements
- SAR approval workflow enforcement with appropriate authority levels
- SAR confidentiality access controls restricting knowledge of SAR existence and content
- Continuing activity tracking and refiling obligation monitoring
- SAR filing confirmation and acknowledgment verification
- SAR filing timeliness metrics and trend reporting
- SAR quality calibration exercises across filing teams
- SAR record retention and destruction schedule compliance

## 11. Typical Systems/Providers

### Typical systems
- SAR filing and preparation platforms
- Case management systems (for investigation-to-SAR handoff)
- SAR narrative generation and assembly tools
- BSA E-Filing gateway and regulatory filing systems
- SAR tracking and status dashboards
- SAR recordkeeping and retention systems

### Typical providers
- NICE Actimize
- Verafin
- Oracle Financial Services
- Hummingbird
- FinCEN BSA E-Filing (regulatory filing gateway)
- WorkFusion (AI-assisted narrative drafting)
- In-house SAR filing platforms

## 12. Open Questions
- Should SAR filing be modeled as a distinct capability from case investigation given their tight operational coupling, or should the boundary be at the point of case determination?
- How should AI-assisted SAR narrative generation be governed within this capability as institutions adopt more automation for narrative drafting?
- Where should the boundary sit between SAR filing (US-specific) and broader suspicious transaction reporting obligations in multi-jurisdictional institutions?
- How should this capability handle joint SARs involving multiple institutions or multi-subject filings that span organizational boundaries?
- Should continuing activity monitoring for refiling be part of this capability or treated as part of ongoing surveillance capabilities?

## 13. Provenance

This capability reflects the critical regulatory reporting obligation that represents one of the primary outputs of the financial crime compliance program. Suspicious activity reporting is modeled as a distinct capability because it has unique regulatory requirements -- specific filing formats, mandated timeframes, strict confidentiality obligations, and continuing activity refiling requirements -- that differ from upstream investigation (analytical and judgment-intensive) and require specialized skills in narrative drafting, regulatory compliance, and filing system operations. The capability is essential to the institution's fulfillment of its BSA/AML obligations and represents a primary touchpoint between the institution and regulatory/law enforcement authorities. The quality and timeliness of SAR filings is one of the most closely scrutinized elements of financial crime compliance programs during regulatory examinations.
