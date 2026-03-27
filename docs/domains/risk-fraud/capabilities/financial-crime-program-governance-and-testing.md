---
id: cap.financial_crime_program_governance_and_testing
type: capability
name: Financial Crime Program Governance & Testing
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Financial Crime Program Governance & Testing

## 1. Definition

Financial Crime Program Governance & Testing is the enduring business ability to govern, oversee, and independently test the institution's financial crime compliance program -- including BSA/AML, fraud prevention, and sanctions compliance programs -- through independent testing and audit, model validation and governance, regulatory change management, training administration, policy and procedure management, and program effectiveness assessment to ensure ongoing regulatory compliance, program adequacy, and continuous improvement.

## 2. Business Outcome

Ensure the institution maintains a well-governed, independently tested, and continuously improving financial crime compliance program that meets regulatory expectations for BSA/AML, fraud, and sanctions compliance, identifies and remediates program deficiencies before they result in regulatory findings, keeps pace with evolving regulatory requirements, maintains competent and well-trained staff, and demonstrates effective board and senior management oversight of the program.

## 3. Scope

### Included activities
- Planning and executing independent testing of the financial crime compliance program across all pillars (BSA/AML, fraud, sanctions)
- Documenting testing findings, deficiencies, and recommendations
- Governing and validating models used in financial crime detection, scoring, and decisioning
- Assessing the impact of regulatory changes on the financial crime program and coordinating implementation
- Managing financial crime policies, procedures, and standards through the governance lifecycle
- Administering financial crime training programs and tracking completion and competency
- Assessing overall program effectiveness through metrics, benchmarking, and trend analysis
- Preparing and presenting board and committee reporting on program status, findings, and risks
- Coordinating with internal audit, external audit, and regulatory examiners on program assessments

### Excluded activities
- Day-to-day operation of detection, investigation, and reporting capabilities (respective operational capabilities)
- Execution of remediation actions for identified deficiencies (`cap.compliance_issue_and_remediation_management`)
- Enterprise-wide compliance program governance outside financial crime scope
- Enterprise model risk management beyond financial crime models
- Enterprise training administration beyond financial crime topics
- IT general controls and cybersecurity governance outside financial crime systems

## 4. Upstream Dependencies
- Operational metrics and performance data from all financial crime capabilities (detection volumes, alert rates, investigation outcomes, SAR filing statistics)
- Model performance data and validation results from detection and scoring capabilities
- Regulatory change notifications and guidance from regulatory bodies (FinCEN, OCC, Federal Reserve, etc.)
- Internal audit findings and assessments
- External audit and regulatory examination findings and commitments
- Industry benchmarking data and peer comparisons

## 5. Downstream Dependencies
- `cap.compliance_issue_and_remediation_management` for tracking and remediating identified deficiencies
- All operational financial crime capabilities for policy, procedure, and standard updates
- Detection and scoring capabilities for model governance decisions and validation requirements
- All financial crime staff for training requirements and competency standards
- Board and senior management for program oversight reporting and risk appetite decisions
- `cap.financial_crime_risk_appetite_and_policy_management` for policy updates informed by testing findings

## 6. Related Value Streams
- `vs.regulatory_compliance`
- `vs.risk_management`
- `vs.operational_efficiency`

## 7. Related Journeys
- `journey.independent_testing_and_audit`
- `journey.model_validation_and_governance`
- `journey.regulatory_change_assessment`
- `journey.training_and_competency_management`
- `journey.program_effectiveness_assessment`

## 8. Likely Processes
- Independent testing planning, scoping, and risk-based scheduling
- Independent testing execution and workpaper documentation
- Testing findings documentation, severity rating, and recommendation development
- Testing results reporting to management, board, and committees
- Model validation planning, execution, and findings documentation
- Model performance monitoring and ongoing governance
- Regulatory change identification, impact assessment, and implementation tracking
- Policy and procedure drafting, review, approval, and version management
- Training curriculum development, delivery, and completion tracking
- Training effectiveness assessment and competency testing
- Program effectiveness metrics definition, collection, and analysis
- Board and committee reporting preparation and presentation
- Regulatory examination coordination and document production
- Peer benchmarking and industry best practice assessment

## 9. Risks
- Program deficiencies undetected by testing result in regulatory findings and enforcement actions
- Model degradation unidentified by validation leads to ineffective detection and increased financial crime risk
- Regulatory changes not timely implemented create compliance gaps and examination exposure
- Inadequate staff training and competency result in operational errors and inconsistent program execution
- Insufficient board and management oversight fails to meet regulatory expectations for program governance
- Testing scope gaps leave material program areas unexamined and unvalidated
- Policy and procedure staleness creates misalignment between documented standards and actual practice
- Inability to demonstrate program effectiveness undermines regulatory confidence in the institution

## 10. Controls
- Independent testing coverage and frequency aligned with risk-based assessment
- Testing finding remediation tracking with escalation for overdue items
- Model validation schedule adherence monitoring
- Regulatory change implementation tracking with compliance deadline monitoring
- Training completion and competency monitoring with escalation for non-compliance
- Board reporting timeliness and completeness standards
- Policy and procedure review cycle compliance monitoring
- Testing independence and objectivity safeguards
- Program effectiveness metrics threshold monitoring and trend analysis
- Regulatory examination commitment tracking and fulfillment verification

## 11. Typical Systems/Providers

### Typical systems
- Governance, risk, and compliance (GRC) platforms
- Testing and audit management systems
- Model risk management platforms
- Training and learning management systems
- Regulatory change management systems
- Policy and procedure management systems
- Board reporting and governance portals

### Typical providers
- Deloitte (independent testing and advisory)
- KPMG (independent testing and advisory)
- PwC (independent testing and advisory)
- Ernst & Young (independent testing and advisory)
- Promontory Financial Group (regulatory advisory)
- ServiceNow (GRC platform)
- Archer (GRC platform)
- MetricStream (GRC platform)
- Cornerstone OnDemand (training management)

## 12. Open Questions
- Should program governance and independent testing be modeled as separate capabilities given their different operational cadences and organizational ownership (testing often performed by internal audit or independent compliance testing teams)?
- How should model governance for financial crime models be coordinated with enterprise model risk management functions and frameworks?
- Where should the boundary sit between financial crime program governance and enterprise-wide compliance program governance?
- How should this capability accommodate the governance of emerging technologies (AI/ML, automation) being deployed across the financial crime program?
- Should regulatory examination management be part of this capability or modeled separately given its cross-cutting nature across all financial crime capabilities?

## 13. Provenance

This capability reflects the critical governance and oversight functions that ensure the overall health, effectiveness, and regulatory compliance of the financial crime program. Program governance and testing is modeled as a distinct capability because it operates at a different organizational level than operational capabilities -- providing independent assessment, oversight, and continuous improvement rather than day-to-day detection, investigation, or reporting. The capability is essential for meeting the regulatory expectation of a well-governed BSA/AML program with independent testing, competent personnel, and effective board oversight. Regulatory examiners consistently identify governance deficiencies (inadequate testing, insufficient training, stale policies, weak board oversight) as root causes of broader program failures, making this capability foundational to overall program health.
