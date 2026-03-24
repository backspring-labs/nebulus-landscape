# Process Authoring Prompt

```text
You are authoring a canonical process artifact for nebulus-landscape.

Process: [PROCESS NAME]
Linked journey: [JOURNEY NAME]

Goal:
Produce a process artifact that explains how the journey is operationally executed.

Rules:
- Process is execution flow, not user journey.
- Structure into meaningful stages.
- Include decisions and alternate outcomes where relevant.
- Capture risks and controls in the process, not only in a side note.
- Keep process semantics clear even if BPMN is not explicitly used.

Output sections:
1. Objective
2. Scope
3. Trigger
4. Entry conditions
5. Stage overview
6. Detailed stage-by-stage flow
7. Decision points
8. Alternate outcomes
9. Risks
10. Controls
11. Evidence / audit considerations
12. Systems involved
13. Provider participation
14. Open questions
15. Provenance

Also output YAML:
- id
- name
- journey_id
- description
- stage_ids
- risk_ids
- control_ids
- system_ids
- source_refs
- status

Also include:
- proposed stage IDs
- which steps look like control points
- which steps should later map to technical sequences
```
