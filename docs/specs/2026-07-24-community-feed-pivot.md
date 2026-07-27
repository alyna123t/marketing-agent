# Direction update — community feed first, then X

*Recorded by: Alyna · Date: 2026-07-24 · Source: Adrian feedback on the repo*

## What changed

We're starting **simpler than the full marketing agent**: a small content feed
for our own community first, then expanding to X.

## The three starting formats (already chosen by the team)

Borrowed from how the Aeon project runs its community feed (Aeon's "Agent Feed"
TG group, `@aeon_agent`):

1. **Weekly shiplog** — what we shipped that week.
2. **Tweet digest** — a digest of tweets.
3. **"Feature built" deep-dive** — one ship, gone deep.

The format side is considered in good shape on the team's end. No need to design
these formats or match brand voice — **brand-voice matching is now team-owned,
not the agent's job.**

## Impact on the existing build

- **`format-receipts-post` is paused** (kept intact, not deleted) — see the banner
  in its `SKILL.md` and the status in `skills/marketing/SKILLS.md`.
- The PRD's phased X plan is deferred behind the community-feed step. The
  single-agent-many-skills architecture is unchanged.

## Next work: X-account audit (research)

The highest-leverage next step is research: audit the best accounts in our space
on X — the same move we made studying the Aeon feed, one level wider — to get our
next set of format ideas and a map of who's doing X well before we post there.

Brief: `docs/superpowers/specs/2026-07-24-x-account-audit-design.md`
Deliverable: `shared-knowledge/marketing/research/x-account-audit.md`
