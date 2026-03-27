---
id: cap.compliance_issue_and_remediation_management
type: capability
name: Compliance Issue & Remediation Management
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Compliance Issue & Remediation Management

## 1. Definition

Compliance Issue & Remediation Management is the enduring business ability to identify, log, classify, track, escalate, and remediate compliance issues, deficiencies, and findings arising from independent testing, regulatory examinations, internal and external audits, and self-identified gaps across the financial crime compliance program, ensuring timely root cause analysis, sustainable corrective action, effective remediation validation, and ongoing tracking of regulatory commitments and matters requiring attention (MRAs).

## 2. Business Outcome

Ensure the institution can systematically identify and remediate compliance deficiencies before they result in regulatory enforcement actions, address root causes rather than symptoms to prevent recurring issues, meet all committed remediation timelines and regulatory commitments, demonstrate continuous improvement of the financial crime program through effective issue management, and maintain regulatory confidence through transparent and well-governed remediation processes.

## 3. Scope

### Included activities
- Identifying and logging compliance issues from independent testing, regulatory examinations, audits, and self-assessment
- Classifying issues by severity, risk impact, and regulatory significance
- Performing root cause analysis to identify underlying drivers of deficiencies
- Developing remediation plans with defined milestones, owners, and timelines
- Obtaining management approval for remediation plans and resource commitments
- Executing remediation actions and tracking milestone completion
- Validating remediation effectiveness through independent testing or verification
- Closing issues with documented evidence of remediation and validation
- Tracking regulatory commitments, MRAs, and consent order requirements
- Escalating overdue, at-risk, or stalled remediation efforts
- Reporting issue inventory, remediation status, and trends to management and board

### Excluded activities
- Independent testing and audit execution that identifies issues (`cap.financial_crime_program_governance_and_testing`)
- Day-to-day operational processes that may be the subject of remediation (respective operational capabilities)
- Enterprise-wide issue and remediation management outside financial crime scope
- Legal dispute resolution and litigation management
- Technology change management and release management for system remediation (technology domain)
- Customer remediation and restitution programs (`domain.servicing`)

## 4. Upstream Dependencies
- Testing findings and deficiency reports from `cap.financial_crime_program_governance_and_testing`
- Regulatory examination findings, MRAs, and consent order requirements from regulatory authorities
- Internal audit findings and recommendations
- External audit findings and management letter items
- Self-identified gaps and issues from operational capabilities across the financial crime program
- Quality assurance findings from operational capabilities (alert triage, investigation, SAR filing)

## 5. Downstream Dependencies
- All financial crime operational capabilities for execution of remediation actions
- `cap.financial_crime_program_governance_and_testing` for remediation validation and effectiveness testing
- Technology and operations teams for system and process changes required by remediation plans
- Board and senior management for remediation status reporting and escalation decisions
- Regulatory authorities for commitment tracking, status updates, and examination responses
- `cap.financial_crime_risk_appetite_and_policy_management` for policy updates driven by issue remediation

## 6. Related Value Streams
- `vs.regulatory_compliance`
- `vs.risk_management`
- `vs.operational_efficiency`

## 7. Related Journeys
- `journey.issue_identification_and_logging`
- `journey.issue_root_cause_analysis`
- `journey.remediation_planning_and_execution`
- `journey.issue_validation_and_closure`
- `journey.regulatory_commitment_tracking`

## 8. Likely Processes
- Issue intake, logging, and classification by severity and risk impact
- Issue severity assessment and prioritization for remediation sequencing
- Root cause analysis and documentation for identified deficiencies
- Remediation plan development, approval, and resource allocation
- Remediation execution and milestone tracking against committed timelines
- Remediation validation and effectiveness testing through independent verification
- Issue closure documentation and evidence archival
- Regulatory commitment and MRA tracking with deadline monitoring
- Issue escalation for overdue, at-risk, or stalled remediation
- Issue inventory reporting and trend analysis for management and board
- Recurring issue pattern analysis and systemic weakness identification
- Consent order compliance tracking and reporting
- Remediation resource and capacity planning

## 9. Risks
- Remediation not completed within committed timelines creates regulatory enforcement exposure and erodes regulatory confidence
- Root cause not adequately addressed results in superficial fixes that fail to prevent recurrence
- Recurring issues indicate systemic weakness that the issue management process is not effectively addressing
- Regulatory commitment breach on MRAs or consent order requirements triggers escalated enforcement actions
- Insufficient remediation validation fails to confirm that corrective actions are effective and sustainable
- Issue inventory growth outpaces remediation capacity, creating a backlog that overwhelms the program
- Inadequate issue classification understates severity and results in insufficient management attention
- Loss of institutional knowledge about issue context and remediation rationale as personnel change

## 10. Controls
- Issue aging and deadline monitoring with escalation thresholds for approaching and overdue items
- Remediation milestone tracking with status reporting and variance analysis
- Issue escalation threshold enforcement based on severity, aging, and risk impact
- Remediation effectiveness validation through independent testing or verification before closure
- Recurring issue pattern analysis to identify systemic weaknesses requiring broader program changes
- Regulatory commitment deadline tracking with early warning and escalation
- Issue classification review and calibration for severity consistency
- Remediation plan approval requirements based on issue severity and regulatory significance
- Issue inventory capacity monitoring and resource allocation governance
- Board and committee reporting on issue inventory, remediation progress, and overdue items

## 11. Typical Systems/Providers

### Typical systems
- Issue management platforms
- Governance, risk, and compliance (GRC) platforms
- Remediation tracking and workflow systems
- Regulatory commitment and MRA tracking systems
- Issue analytics and reporting dashboards
- Document management systems for remediation evidence

### Typical providers
- ServiceNow (GRC and issue management)
- Archer (GRC and issue management)
- MetricStream (GRC and issue management)
- IBM OpenPages (GRC platform)
- Workiva (regulatory reporting and compliance)
- In-house issue management and tracking platforms

## 12. Open Questions
- Should compliance issue and remediation management be modeled as a financial-crime-specific capability or as an enterprise-wide capability that serves financial crime among other compliance domains?
- How should the boundary between issue management (tracking and governance) and remediation execution (actual operational changes) be defined in the capability model?
- Where should technology remediation (system changes, configuration updates, data fixes) be governed when it involves both financial crime compliance and technology teams?
- How should this capability handle cross-cutting issues that span multiple financial crime capabilities or require coordinated remediation across organizational boundaries?
- Should regulatory commitment tracking (MRAs, consent orders) be a distinct capability given its unique governance requirements and regulatory significance?

## 13. Provenance

This capability reflects the essential function of closing the loop on identified deficiencies to ensure continuous improvement and regulatory compliance of the financial crime program. Compliance issue and remediation management is modeled as a distinct capability because it requires specific governance processes -- root cause analysis, remediation planning, milestone tracking, effectiveness validation, and regulatory commitment management -- that differ from both the testing function that identifies issues and the operational functions that execute remediation actions. Regulatory examiners consistently evaluate the effectiveness of issue management as a key indicator of program health, and recurring or unresolved issues are among the most common drivers of escalated regulatory enforcement. The capability serves as the connective tissue between program governance oversight and operational improvement execution.
