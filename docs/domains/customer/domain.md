---
id: domain.customer
type: domain
name: Customer
status: draft
source_refs:
  - src.domain_map_v1
last_reviewed: 2026-03-24
confidence: medium
---

# Customer

## Definition

The Customer domain encompasses the business area responsible for acquiring, identifying, onboarding, and maintaining customer relationships. It covers the lifecycle from initial prospect engagement through active relationship management, including identity establishment, profile maintenance, and consent governance.

## Business Purpose

Enable the institution to establish and maintain trusted customer relationships. This domain is the entry point for nearly all retail and small business banking journeys — without a well-functioning Customer domain, downstream capabilities like account opening, payments, and lending cannot operate.

## Scope

Includes:
- Customer acquisition and onboarding
- Customer identity establishment and verification
- Customer profile creation and maintenance
- Consent and preference management
- Customer lifecycle state management

Excludes:
- Authentication and session management (→ Identity & Access)
- Account creation and management (→ Accounts)
- Transaction processing (→ Payments)
- Credit decisioning (→ Risk & Fraud)

## Included Capabilities

- `cap.customer_onboarding` — Initiate, collect, validate, and progress a new customer through onboarding
- `cap.customer_profile` — Create, maintain, and govern the customer profile and identity record
- `cap.consent_management` — Manage customer consent, preferences, and data-sharing agreements

## Excluded Capabilities

- Authentication (belongs to Identity & Access)
- Account opening (belongs to Accounts, though tightly linked via journey)
- Fraud decisioning (belongs to Risk & Fraud, though participates in onboarding)

## Upstream Dependencies

- Marketing / lead generation (external to current model scope)
- Channel presentation (→ Channels & Experience)

## Downstream Dependencies

- `domain.identity_access` — Identity verification during onboarding
- `domain.accounts` — Account opening after customer is established
- `domain.risk_fraud` — Risk evaluation during onboarding
- `domain.payments` — Funding and money movement after account creation

## Related Journeys

- `journey.open_savings_account` — Flagship journey spanning Customer, Identity & Access, and Accounts

## Typical Risks and Controls

- `risk.synthetic_identity` — Fabricated identity used to open an account
- `ctrl.idv_before_funding` — Identity must be verified before funding is permitted
- Incomplete or fraudulent application data
- Consent violations or data-sharing non-compliance

## Typical Systems and Providers

- `sys.identity_service` — Manages identity verification interactions
- `prov.alloy` — Identity, fraud, and onboarding orchestration
- Mobile and web applications (channel entry points)
- CRM / customer master data systems

## Open Questions

- Where is the boundary between customer profile management and identity management? Some institutions merge these; others keep them strictly separate.
- How should consent management relate to data governance capabilities that may emerge in a future Data & Analytics domain?
- Should customer lifecycle state (prospect → applicant → active → dormant → closed) be modeled as a separate capability or as part of customer profile?

## Provenance

Initial domain artifact authored as part of the nebulus-landscape Phase 1 bootstrap. Based on common banking/fintech domain models and the canonical model idea documents. Confidence is medium — the domain boundaries are well-understood in the industry but the specific capability decomposition reflects one reasonable interpretation.
