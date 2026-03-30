---
id: ref.domains
type: reference_taxonomy
name: Domain Index
status: draft
source_refs:
  - src.domain_map_v1
last_reviewed: 2026-03-24
---

# Domain Index

Planned domains for the nebulus-landscape canonical model, in recommended bootstrapping order.

| # | Domain | ID | Status |
|---|--------|----|--------|
| 1 | Customer | `domain.customer` | Complete (18 capabilities, 4 journeys, 4 processes) |
| 2 | Identity & Access | `domain.identity_access` | Complete (17 capabilities, 4 journeys, 4 processes) |
| 3 | Accounts | `domain.accounts` | Complete (17 capabilities, 4 journeys, 4 processes) |
| 4 | Payments | `domain.payments` | Complete (18 capabilities, 4 journeys, 4 processes) |
| 5 | Risk & Fraud | `domain.risk_fraud` | Complete (17 capabilities, 4 journeys, 4 processes) |
| 6 | Orchestration & Control | `domain.orchestration_control` | Planned |
| 7 | Channels & Experience | `domain.channels_experience` | Planned |
| 8 | Cards | `domain.cards` | Planned |
| 9 | Networks & Schemes | `domain.networks_schemes` | Planned |
| 10 | Data & Rewards | `domain.data_rewards` | Planned |
| 11 | Servicing | `domain.servicing` | Planned |
| 12 | Ledger / Core Banking | `domain.ledger_core` | Planned |

## Notes

- This order gives a strong flagship journey set early (Customer + Identity + Accounts enables the Open Savings Account journey).
- Domains 1–3 are scaffolded in Phase 1. Domains 4–12 will be added domain-by-domain in subsequent phases.
- Domain boundaries may shift as capabilities are decomposed — this index should be updated during reconciliation passes.
