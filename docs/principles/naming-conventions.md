# Naming Conventions

## Stable ID strategy

IDs are the spine that connects `nebulus-landscape` → `nebulus-financial` → `guiderail`. They must be stable, canonical, and never silently renamed once referenced.

### ID patterns

| Object Type | Pattern | Example |
|-------------|---------|---------|
| Domain | `domain.<name>` | `domain.customer` |
| Capability | `cap.<name>` | `cap.customer_onboarding` |
| Value Stream | `vs.<name>` | `vs.acquire_and_activate_customer` |
| Journey | `journey.<name>` | `journey.open_savings_account` |
| Process | `proc.<name>_v<N>` | `proc.account_opening_v1` |
| Process Stage | `stage.<process>.<name>` | `stage.account_opening.verify_identity` |
| Risk | `risk.<name>` | `risk.synthetic_identity` |
| Control | `ctrl.<name>` | `ctrl.idv_before_funding` |
| Provider | `prov.<name>` | `prov.alloy` |
| Provider Mapping | `map.prov.<name>` | `map.prov.alloy` |
| Provider Research | `research.prov.<name>` | `research.prov.alloy` |
| System | `sys.<name>` | `sys.identity_service` |
| Interface | `if.<system>.<name>` | `if.identity_service.verify_identity` |
| Message | `msg.<name>` | `msg.identity_verification_request` |
| Evidence | `ev.<name>` | `ev.idv_policy_v1` |
| Source | `src.<name>` | `src.provider.alloy.docs` |

### ID rules

- Use `snake_case` for all ID name segments
- IDs must not be renamed once referenced by other artifacts or external repos
- To deprecate an ID, mark the artifact `status: deprecated` and create a new artifact with the new ID
- Version suffixes (`_v1`, `_v2`) are used for process and evidence artifacts where explicit versioning matters
- Keep names concise but specific — `cap.onboarding` is too vague; `cap.customer_onboarding` is right

## File naming

### YAML model files
Named after the canonical ID: `<id>.yaml`
- `domain.customer.yaml`
- `cap.customer_onboarding.yaml`
- `proc.account_opening_v1.yaml`
- `stage.account_opening.verify_identity.yaml`

### Markdown doc files
Use `kebab-case`: `<descriptive-name>.md`
- `customer-onboarding.md`
- `open-savings-account.md`

### Directories
Use `kebab-case`:
- `docs/domains/customer/`
- `docs/cross-domain/value-streams/`
- `models/process-stages/`

## Frontmatter requirements

All markdown artifacts must include these required fields:

```yaml
---
id: <canonical ID>
type: <domain | capability | value_stream | journey | process | process_stage | risk | control | provider | system>
name: <display name>
status: <draft | active | deprecated | placeholder>
source_refs: []
last_reviewed: <YYYY-MM-DD>
confidence: <high | medium | low | candidate | disputed>  # optional in early phases
---
```

Type-specific optional fields:
- `domain_id` — for capabilities, systems
- `capability_ids` — for journeys, processes, providers
- `journey_id` — for processes
- `process_id` — for process stages
