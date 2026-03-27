---
id: cap.fraud_rules_and_model_governance
type: capability
name: Fraud Rules & Model Governance
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Fraud Rules & Model Governance

## 1. Definition

Fraud Rules & Model Governance is the enduring business ability to govern the full lifecycle of fraud detection rules, machine learning models, scoring thresholds, and analytical parameters through structured development, validation, deployment, monitoring, and retirement processes to ensure detection effectiveness, regulatory compliance, and appropriate model risk management.

## 2. Business Outcome

Ensure the institution's fraud detection models and rules remain effective against evolving fraud typologies, comply with model risk management regulatory requirements (SR 11-7 / OCC 2011-12), maintain documented governance trails, and operate within defined performance parameters while enabling timely model and rule updates to address emerging threats.

## 3. Scope

### Included activities
- Governing the development lifecycle for fraud detection models including requirements, development, documentation, and approval
- Validating fraud models through independent review, back-testing, and challenger analysis prior to production deployment
- Managing fraud model deployment governance including change approval, rollback procedures, and production monitoring
- Monitoring fraud model performance in production including detection rates, false positive rates, model drift, and score distribution stability
- Governing fraud rule development, testing, approval, and deployment
- Maintaining the fraud model and rule inventory with documentation, ownership, and lifecycle status
- Managing fraud detection threshold calibration and tuning governance
- Coordinating model retirement and replacement when models are no longer effective
- Ensuring compliance with model risk management regulatory frameworks

### Excluded activities
- Fraud detection execution and scoring in production (`cap.fraud_detection_and_scoring`)
- Transaction monitoring scenario execution (`cap.transaction_monitoring`)
- Alert triage and disposition (`cap.alert_triage_and_disposition`)
- Financial crime risk appetite and policy definition (`cap.financial_crime_risk_appetite_and_policy_management`)
- General enterprise model risk management for non-fraud models (enterprise model risk capabilities)
- Data science and analytics infrastructure management (technology/data capabilities)
- Fraud investigation and case management (investigation capabilities)

## 4. Upstream Dependencies
- Fraud detection performance data from `cap.fraud_detection_and_scoring` for model monitoring
- Confirmed fraud outcomes from investigation capabilities for model back-testing and validation
- False positive data from `cap.alert_triage_and_disposition` for model tuning feedback
- Fraud typology intelligence from industry sources, consortium data, and internal trend analysis
- Risk appetite and policy parameters from `cap.financial_crime_risk_appetite_and_policy_management`
- Model risk management framework requirements from enterprise model governance
- Historical transaction and fraud data for model development and validation

## 5. Downstream Dependencies
- `cap.fraud_detection_and_scoring` for production deployment of validated models and rules
- `cap.payment_and_transfer_risk_decisioning` for risk scoring model updates
- Enterprise model risk management for fraud model inventory reporting
- Regulatory reporting for model governance compliance documentation
- Internal audit for model governance audit evidence
- Management reporting for model performance and governance metrics

## 6. Related Value Streams
- `vs.fraud_prevention`
- `vs.financial_crime_detection`
- `vs.regulatory_compliance`
- `vs.operational_efficiency`

## 7. Related Journeys
- `journey.fraud_rule_change_request`
- `journey.fraud_model_development_and_validation`
- `journey.fraud_model_production_deployment`
- `journey.fraud_model_performance_review`
- `journey.fraud_rule_retirement`

## 8. Likely Processes
- Fraud model development request and requirements definition
- Fraud model development and documentation
- Independent fraud model validation and challenger testing
- Fraud model deployment approval and production release
- Fraud model performance monitoring and drift detection
- Fraud rule development, testing, and approval
- Fraud rule and threshold tuning and calibration
- Fraud model and rule inventory management and documentation maintenance
- Fraud model retirement and replacement governance
- Model risk management regulatory compliance reporting
- Fraud model back-testing and outcome analysis
- Fraud typology research and model enhancement prioritization

## 9. Risks
- Unvalidated fraud models in production produce unreliable scores and detection gaps
- Fraud model drift goes undetected, gradually degrading detection effectiveness
- Unauthorized or inadequately tested fraud rule changes disrupt detection or create false positive spikes
- Fraud rule conflicts or redundancies create unpredictable scoring behavior
- Inadequate fraud model documentation fails model risk management regulatory requirements
- Model governance process delays prevent timely response to emerging fraud typologies
- Over-reliance on legacy rules without model-based approaches misses novel fraud patterns
- Insufficient segregation between model development and validation compromises independence

## 10. Controls
- Fraud model validation requirement before production deployment
- Fraud model performance monitoring cadence with defined review triggers
- Fraud rule change approval workflow with testing and documentation requirements
- Fraud model inventory completeness and accuracy review
- Fraud model documentation standards compliance checks
- Segregation of duties between model development, validation, and production deployment
- Model drift detection thresholds with automated alerting
- Fraud rule conflict and overlap detection
- Model retirement governance with defined criteria and approval requirements
- Regulatory compliance review for model risk management framework adherence

## 11. Typical Systems/Providers

### Typical systems
- Fraud model governance and lifecycle management platforms
- Fraud rule management and configuration systems
- Model validation and back-testing environments
- Model performance monitoring dashboards
- Model inventory and documentation repositories
- MLOps platforms for model deployment and monitoring
- Rule testing and simulation environments

### Typical providers
- FICO
- Featurespace
- SAS
- DataRobot
- NICE Actimize
- Domino Data Lab
- In-house model governance platforms

## 12. Open Questions
- Should fraud model governance and AML/transaction monitoring model governance be combined into a single financial crime model governance capability?
- How should this capability relate to enterprise-wide model risk management when fraud models are a subset of the institution's model inventory?
- Where should the boundary sit between fraud model governance (lifecycle management) and fraud detection operations (production execution)?
- How should the governance of AI/ML-based fraud models differ from traditional rules-based detection governance given their different explainability and validation characteristics?
- Should vendor-provided fraud models (e.g., consortium models, vendor black-box models) be governed differently than internally developed models within this capability?
- How should this capability address the tension between governance rigor and the speed needed to respond to rapidly evolving fraud typologies?

## 13. Provenance

This capability reflects the model risk management requirements that apply to fraud detection models and rules, driven primarily by SR 11-7 / OCC 2011-12 and the increasing sophistication of fraud detection approaches. It is modeled separately from fraud detection operations because governance has distinct process requirements (validation independence, documentation standards, change approval workflows) and different organizational stakeholders (model risk management, internal audit, regulators) than production detection operations. The capability encompasses both traditional rules and ML models because both require structured governance, though their validation and monitoring approaches differ. The growing use of AI/ML in fraud detection makes this governance capability increasingly critical as regulators focus on model explainability, fairness, and ongoing monitoring requirements.
