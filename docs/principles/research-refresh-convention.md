# Research Refresh Convention

## Purpose

The landscape model is not a one-time authoring exercise. Domains, capabilities, providers, and practices evolve. This convention defines how to incrementally refresh research without losing canonical model integrity.

## Workflow

### 1. Create a branch

All research refreshes happen on a branch, not directly on main. This allows review before canonical model changes are merged.

Branch naming: `refresh/<domain>-<YYYY-MM>` (e.g., `refresh/customer-2026-06`)

### 2. Conduct research

Review the current state of the domain or topic against:
- new provider developments
- industry changes
- internal learning from financial implementation
- feedback from guiderail usage
- open questions flagged on existing artifacts

### 3. Write a research log entry

Create a dated entry in `docs/research-log/`:

```
docs/research-log/YYYY-MM-<domain-or-topic>.md
```

The entry should capture:
- what was reviewed
- what changed or was newly discovered
- what remains uncertain
- which canonical artifacts are affected

### 4. Produce one of three outcomes

Every refresh must produce one of these outcomes for each finding:

| Outcome | Meaning | Action |
|---------|---------|--------|
| **Canonical model updated** | Findings are strong enough to change canonical objects | Update the relevant YAML and doc artifacts on the branch |
| **Research logged but not promoted** | Findings are real but not yet ready for canonical status | Capture in the research-log entry only; do not modify canonical objects |
| **Ambiguity recorded** | Uncertainty discovered or deepened | Add or update an "Open Questions" entry on the relevant artifact |

### 5. Review and merge

Open a PR from the refresh branch. Review should check:
- Are promoted changes well-sourced?
- Are confidence levels updated where applicable?
- Are `last_reviewed` dates updated on touched artifacts?
- Are open questions captured for unresolved findings?
- Does the research-log entry accurately summarize the refresh?

### 6. Update `last_reviewed`

All artifacts that were reviewed during the refresh (whether changed or not) should have their `last_reviewed` date updated.

## Anti-patterns

- **Vague journaling** — a research-log entry that says "looked at payments, some stuff changed" is not useful. Be specific about what was found and what it means.
- **Silent canonical changes** — changing a YAML model without a research-log entry explaining why removes provenance.
- **Refresh without outcomes** — if you reviewed something and found nothing changed, that is still worth a brief log entry ("reviewed, no changes needed").
