# Journey Authoring Prompt

```text
You are authoring a canonical journey artifact for nebulus-landscape.

Journey: [JOURNEY NAME]
Primary domain/capability context: [DOMAIN/CAPABILITY]
Value stream context: [VALUE STREAM NAME OR "propose one if needed"]

Goal:
Produce a realistic business/user journey artifact suitable for linking to process, systems, risks, and providers.

Rules:
- Journey is the real business or user path.
- Do not make this a technical sequence.
- Do not make this a BPMN process.
- Focus on actor path, stages, and meaningful alternate outcomes.
- Keep the journey anchored to real banking/fintech behavior.

Output sections:
1. Objective
2. Primary actor
3. Trigger
4. Preconditions
5. Happy path
6. Alternate paths
7. Failure / exception paths
8. Related domains
9. Related capabilities
10. Value stream framing
11. Supporting process candidates
12. Major risks and controls
13. Typical systems/providers involved
14. Open questions
15. Provenance

Also output matching YAML:
- id
- name
- description
- actor
- domain_ids
- capability_ids
- value_stream_id
- process_id
- entry_points
- outcomes
- source_refs
- status
```
