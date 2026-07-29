# drafts/community-feed — the review queue

This folder **is** the Tier-2 queue until Paperclip is wired. The agent writes finished
drafts here; a human does the final paste to the feed (draft-only, per PRD §6).

- **Naming:** `YYYY-WW-shiplog.md`, `YYYY-WW-digest.md`, `YYYY-WW-feature-<slug>.md`.
- **Contents:** the skill's full output schema — body, tier, claim→evidence map, gate —
  not just the post text. The map is what review checks.
- **Gate failures land here too.** A file whose `Gate:` line reads "no post — nothing
  cleared the gate" is a normal artifact; it's how we track skip rates per format.
- **After review:** posted → move the file to `posted/`; rejected → leave in place with
  a `Rejected:` line and the reason (the monthly retro reads these).
- **Kill condition (AGENT.md):** anything sitting here unreviewed > 14 days in the first
  6 weeks means the loop is broken — revert to manual.
