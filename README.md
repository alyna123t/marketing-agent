# marketing-agent

Working repo for Simmer's marketing agent. Laid out to the `agentcompanies/v1`
shape so it lifts directly into `simmer-labs` when handed off.

**One agent, many skills.** The marketing agent is *not* a new runtime and *not*
a new agent per channel — it is Aeon's existing scheduled runtime pointed at a
skill library. Every new format is a new skill file, never a new agent.

## Layout

```
docs/specs/                     The PRD and future specs
agents/marketing-agent/          Agent config (identity, schedule, tool access)
skills/marketing/               The skill library (the real work)
  _SKILL_TEMPLATE.md            Contract every skill must follow
  format-weekly-shiplog/        Community feed — current phase
  format-tweet-digest/          Community feed — current phase
  format-feature-built/         Community feed — current phase
  format-receipts-post/         X phase — paused, kept intact
  weekly-data-drop-builder/     X phase
shared-knowledge/marketing/     Brand voice, patterns, weekly data drops
  data-drops/                   YYYY-WW.md artifacts (the moat)
  patterns/                     Winning formats promoted from the monthly retro
  research/                     X-account audit (10 accounts), KOL pool
test-fixtures/                  Synthetic source material; RED/GREEN test method
```

## Start here

1. Read [`docs/specs/2026-07-24-community-feed-pivot.md`](docs/specs/2026-07-24-community-feed-pivot.md) — the current direction. The full X plan in [`the PRD`](docs/specs/2026-07-24-marketing-agent-prd.md) is deferred behind it.
2. Current build is the three community-feed formats: `format-weekly-shiplog`, `format-tweet-digest`, `format-feature-built`. See [`skills/marketing/SKILLS.md`](skills/marketing/SKILLS.md).
3. Every skill follows [`skills/marketing/_SKILL_TEMPLATE.md`](skills/marketing/_SKILL_TEMPLATE.md) and is RED/GREEN tested against [`test-fixtures/`](test-fixtures/README.md) before it counts as built.

## Status

Draft for Adrian & Nick. Publishing is **draft-only** for the first 6 weeks
(agent writes finished drafts; a human does the final paste).

**Blocking the community feed going live:** the three format skills are built and
tested, but their inputs aren't wired — there's no ship source (merged PRs? Paperclip?
a changelog?) and no tweet-capture pipeline. Each skill states its assumption in a
`SOURCE ASSUMPTION` block and runs against `test-fixtures/` until then.
