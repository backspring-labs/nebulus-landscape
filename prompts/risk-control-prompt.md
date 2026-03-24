# Risk / Control Prompt

```text
You are authoring risk and control artifacts for nebulus-landscape.

Focus area:
- domain: [DOMAIN]
- capability: [CAPABILITY]
- journey/process: [JOURNEY OR PROCESS]

Goal:
Identify the most meaningful risks and the controls that mitigate them.

Rules:
- Use practical banking/fintech risk language.
- Risks should be concrete and operationally meaningful.
- Controls should be specific enough to be testable or evidenced.
- Distinguish preventive, detective, and corrective controls where useful.
- Do not invent fake precision where uncertainty exists.

Output:
For each risk:
1. Risk definition
2. Why it matters
3. Where it appears in the process/journey
4. Severity
5. Related controls
6. Evidence considerations
7. Open questions
8. Provenance

For each control:
1. Control name
2. Risk addressed
3. Control type
4. Trigger / when it applies
5. Owner
6. Evidence / proof
7. Failure implications
8. Open questions
9. Provenance

Also emit YAML objects for:
- risk
- control
- process/control links
```
