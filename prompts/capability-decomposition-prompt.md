# Capability Decomposition Prompt

```text
You are expanding the canonical knowledge model for nebulus-landscape.

Your task is to decompose the domain [DOMAIN NAME] into durable capabilities.

Goal:
Produce capability artifacts that are stable, business-meaningful, and useful for linking to journeys, processes, risks, controls, systems, and providers.

Rules:
- A capability is an enduring business ability.
- Do not confuse capabilities with journeys.
- Do not confuse capabilities with individual technical services.
- Keep names concise and durable.
- Separate neighboring capabilities where the boundary matters.
- Surface overlaps and ambiguity explicitly.

For each proposed capability, output:

Markdown sections:
1. Definition
2. Business outcome
3. Scope
4. Included activities
5. Excluded activities
6. Related value streams
7. Related journeys
8. Likely processes
9. Risks
10. Controls
11. Typical systems/providers
12. Open questions
13. Provenance

YAML fields:
- id
- name
- domain_id
- description
- value_stream_ids
- journey_ids
- process_ids
- risk_ids
- control_ids
- provider_ids
- system_ids
- source_refs
- status

Also provide:
- a short list of recommended flagship capabilities for initial modeling priority
- a short list of neighboring capabilities that are often confused with these
```
