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
  intake/                       v1 pipeline: weekly human paste of ships + tweets
  patterns/                     Winning formats promoted from the monthly retro
  research/                     X-account audit (10 accounts), KOL pool
drafts/community-feed/          The review queue (agent writes, human pastes)
test-fixtures/                  Synthetic source material; RED/GREEN test method
.github/workflows/              The agent's scheduled runtime (Mon 09:00 UTC)
```

## Start here

1. **Reviewing the v1?** Read [`docs/2026-07-29-community-feed-v1-samples.md`](docs/2026-07-29-community-feed-v1-samples.md) — one sample post per format plus the open knobs. That doc is the format spec, executed.
2. Read [`docs/specs/2026-07-24-community-feed-pivot.md`](docs/specs/2026-07-24-community-feed-pivot.md) — the current direction. The full X plan in [`the PRD`](docs/specs/2026-07-24-marketing-agent-prd.md) is deferred behind it.
3. Current build is the three community-feed formats: `format-weekly-shiplog`, `format-tweet-digest`, `format-feature-built`. See [`skills/marketing/SKILLS.md`](skills/marketing/SKILLS.md).
4. Every skill follows [`skills/marketing/_SKILL_TEMPLATE.md`](skills/marketing/_SKILL_TEMPLATE.md) and is RED/GREEN tested against [`test-fixtures/`](test-fixtures/README.md) before it counts as built.

## Status

Draft for Adrian & Nick. Publishing is **draft-only** for the first 6 weeks
(agent writes finished drafts; a human does the final paste).

**To go live:** (1) Adrian reviews the sample pack; (2) format feedback lands in the
skill files; (3) `ANTHROPIC_API_KEY` secret added to the repo; (4) someone pastes the
first real week into `shared-knowledge/marketing/intake/` and triggers the workflow.
The v1 "pipeline" is that weekly paste — automating the ship/tweet sources is
deliberately deferred (see `intake/README.md`).
