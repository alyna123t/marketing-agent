# SKILL: <name>

*The contract every marketing skill follows. Copy this file into a new skill
folder as `SKILL.md` and fill every section. The contract is what keeps agent
output predictable. Do not leave a section blank.*

## Trigger
When this skill should be loaded/run. Be specific (a schedule, an issue label,
a stage in the pipeline).

## Inputs
The exact files, issues, or artifacts this skill requires. Name paths.
State what to do if an input is missing (see Failure handling).

## Process checklist
Ordered steps the agent follows. One action per line. Include the required
"pause" steps (e.g. confirm audience before drafting).

## Output schema
The exact shape of what this skill produces: markdown structure and/or JSON
sidecar fields. Downstream skills depend on this being stable.

## Quality gates
Pass/fail criteria checked before the output is considered done. E.g. "every
claim has an evidence-map entry" or "exactly one target audience".

## Failure handling
What to do when data is missing, a gate fails, or the input is malformed.
Default: file a Paperclip issue describing the gap; do not fabricate.

## Escalation
When to force human review instead of proceeding (which tier, who).
