# aeon-marketing

Working repo for Simmer's marketing agent. Laid out to the `agentcompanies/v1`
shape so it lifts directly into `simmer-labs` when handed off.

**One agent, many skills.** The marketing agent is *not* a new runtime and *not*
a new agent per channel — it is Aeon's existing scheduled runtime pointed at a
skill library. Every new format is a new skill file, never a new agent.

## Layout

```
docs/specs/                     The PRD and future specs
agents/aeon-marketing/          Agent config (identity, schedule, tool access)
skills/marketing/               The skill library (the real work)
  _SKILL_TEMPLATE.md            Contract every skill must follow
  weekly-data-drop-builder/     Phase 1
  format-receipts-post/         Phase 1
shared-knowledge/marketing/     Brand voice, patterns, weekly data drops
  data-drops/                   YYYY-WW.md artifacts (the moat)
  patterns/                     Winning formats promoted from the monthly retro
```

## Start here

1. Read [`docs/specs/2026-07-24-marketing-agent-prd.md`](docs/specs/2026-07-24-marketing-agent-prd.md).
2. Phase-1 build is two skills: `weekly-data-drop-builder` and `format-receipts-post`.
3. Every skill follows [`skills/marketing/_SKILL_TEMPLATE.md`](skills/marketing/_SKILL_TEMPLATE.md).

## Status

Draft for Adrian & Nick. Publishing is **draft-only** for the first 6 weeks
(agent writes finished drafts; a human does the final paste).
