---
id: ref.value_streams
type: reference_taxonomy
name: Value Stream Index
status: placeholder
source_refs:
  - src.domain_map_v1
last_reviewed: 2026-03-24
---

# Value Stream Index

Value streams are higher-order business flows that frame capabilities and journeys. They represent the end-to-end value-producing paths that the institution operates.

## Planned value streams

| Value Stream | ID | Key Domains | Status |
|--------------|----|-------------|--------|
| Acquire and Activate Customer | `vs.acquire_and_activate_customer` | Customer, Identity & Access, Accounts | Referenced in Phase 1 seed |
| Move Money | `vs.move_money` | Payments, Accounts, Networks & Schemes | Planned |
| Service Customer Account | `vs.service_customer_account` | Servicing, Accounts, Channels & Experience | Planned |

## Notes

- Value streams frame but do not replace capabilities or journeys. See `docs/principles/modeling-principles.md`.
- `vs.acquire_and_activate_customer` is referenced by the Phase 1 Customer seed but does not yet have its own full value stream artifact.
- Additional value streams will be identified as domains are modeled.
