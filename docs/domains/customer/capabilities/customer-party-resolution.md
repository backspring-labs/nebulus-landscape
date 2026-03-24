---
id: cap.customer_party_resolution
type: capability
name: Customer Party Resolution
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Party Resolution

## 1. Definition

Customer Party Resolution is the enduring business ability to determine whether a prospect, applicant, or related party corresponds to an existing customer/party, a new customer/party, or a record requiring manual review, and to govern the match, deduplication, and merge decision lifecycle.

## 2. Business Outcome

Prevent unnecessary duplicate customer creation, enable proper recognition of existing relationships, and ensure that onboarding and downstream servicing operate against the correct customer identity anchor.

## 3. Scope

### Included activities
- Searching and matching incoming prospect/applicant/related-party data against existing customer/party records
- Calculating and governing match confidence and ambiguity thresholds
- Identifying probable duplicates, near matches, household/relationship overlaps, and manual-review candidates
- Supporting merge, link, retain-separate, and unmerge decisions where authorized
- Resolving whether an onboarding flow should continue as new-to-bank or existing-customer
- Managing false-positive/false-negative tradeoffs in customer matching
- Coordinating party resolution for joint applicants, business principals, beneficial owners, and authorized users where relevant
- Preserving provenance of match decisions and steward overrides

### Excluded activities
- Fraud or synthetic identity detection (`domain.risk_fraud`)
- Identity proofing and evidence verification (`domain.identity_access`)
- Creation of the canonical customer record itself (`cap.customer_identity_establishment`)
- Generic MDM platform ownership as enterprise technology
- Account-holder relationship maintenance (`domain.accounts`)
- Ongoing servicing-case management (`domain.servicing`)

## 4. Upstream Dependencies
- Applicant/prospect data from `cap.customer_application_intake` and `cap.customer_acquisition_management`
- Existing party/customer corpus from `cap.customer_identity_establishment`
- Optional verification-quality signals from `domain.identity_access`
- Reference data such as address normalization or name standards from data domains

## 5. Downstream Dependencies
- `cap.customer_identity_establishment` for create-vs-link outcomes
- `cap.customer_onboarding_orchestration` for exception handling on ambiguous matches
- `domain.accounts` when an existing customer is adding another relationship
- `domain.servicing` when duplicate remediation becomes operational work
- `domain.risk_fraud` when suspicious duplication patterns may warrant investigation

## 6. Related Value Streams
- `vs.customer_mastering`
- `vs.customer_onboarding`
- `vs.data_quality`
- `vs.relationship_recognition`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.existing_customer_adds_new_relationship`
- `journey.correct_duplicate_customer_record`
- `journey.open_joint_account`
- `journey.business_principal_onboarding`

## 8. Likely Processes
- Pre-create match screening
- Existing-customer recognition
- Duplicate review
- Merge and link decisioning
- Manual steward review
- False-positive remediation
- Unmerge/remediation handling

## 9. Risks
- Duplicate customers are created because the institution misses a true match
- Distinct customers are incorrectly merged or linked
- Existing customers are misclassified as new, creating duplicate onboarding or incentives
- Manual stewards override with insufficient evidence
- Matching logic is biased toward certain name/address patterns and creates unfair outcomes
- Downstream systems do not honor the resolved party decision consistently
- Household or business-relationship linkages are mistaken for same-party matches

## 10. Controls
- Match-rule governance with documented thresholds
- Gold-standard test cases for false-positive/false-negative monitoring
- Mandatory manual review band for ambiguous matches
- Controlled merge/unmerge authority and evidence requirements
- Steward audit logs and decision rationale capture
- Periodic duplicate-rate and bad-merge monitoring
- Segregation between match-review and fraud-investigation outcomes
- Post-merge downstream reconciliation controls

## 11. Typical Systems/Providers

### Typical systems
- Party matching / entity resolution engines
- MDM matching services
- Stewardship review consoles
- Search APIs for existing-customer lookup
- Data quality and standardization services

### Typical providers
- Informatica
- Reltio
- IBM Match 360
- LexisNexis Risk Solutions
- NICE Actimize data quality / entity context components where applicable
- Experian and TransUnion identity data services where used for matching support
- In-house fuzzy-match and entity-resolution services

## 12. Open Questions
- Should party resolution and canonical identity establishment remain separate capabilities permanently, or only until the model reaches deeper party-master maturity?
- How should householding be related to same-party resolution if the repo later models household/customer-group capabilities?
- Should business-principal and beneficial-owner resolution remain here or move to a broader party-resolution capability outside Customer?
- What level of explainability should be required for probabilistic matching in the canonical model?

## 13. Provenance

This capability is durable in institutions that have learned the cost of duplicate customer records. It is intentionally separated from both fraud detection and proofing because party resolution answers "is this already our customer?" rather than "is this identity genuine?" or "is this activity suspicious?"
