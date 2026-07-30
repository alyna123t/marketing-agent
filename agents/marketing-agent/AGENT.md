# Agent: marketing-agent

*Extends Aeon. Not a new runtime, not a new agent per channel.*

**Current phase: internal community feed** (pivot, 2026-07-24). The X/LinkedIn plan
in the PRD is deferred behind it; those rows below are marked deferred, not deleted.

| Field | Value |
| --- | --- |
| Role | Turn the week's ships, results, and watchlist intel into community-feed drafts |
| Runtime | Claude Code, run by hand. The Actions runner is deliberately not wired and no API key is added: posting is manual until the format proves out on the real feed (Adrian, 2026-07-30) |
| Model | claude-sonnet-5 (skills were GREEN-tested on Sonnet-class; retune as cost/capability shift) |
| Coordination | `drafts/community-feed/` is the review queue until Paperclip is wired |
| Knowledge | reads/writes `shared-knowledge/marketing/`; reads `intel/`, `patterns/` |

## Tool access (matrix, not free-for-all)

| Tool | Access | Notes |
| --- | --- | --- |
| `shared-knowledge/marketing/intake/` | read | The only source of ships and tweets (v1 = human paste; see `intake/README.md`) |
| `drafts/community-feed/` | write | Drafts + blocked notes + gate-failure logs land here |
| Weekly trade artifact / `data-drops/` | read | Internal only. The shiplog is ships-only (2026-07-30): per-agent PnL stays private; the receipts beat returns when Adrian calls a number worth showing |
| Web / X fetching | **none** | Missing intake is a blocked note, never a scrape |
| Community feed posting | **none** | Draft-only; a human does the final paste |
| X / LinkedIn (MCP) | *deferred* | Returns with the X phase, tier-gated per PRD §5 |
| Paperclip | *not wired* | The drafts folder stands in; swap when wired |
| UTM/link tooling | *deferred* | X-phase; community-feed posts carry no CTA by default |

## Operating rules

- Add skills, not agents. Every format is a skill file.
- No claim ships without an evidence map (`claim-evidence-linter` discipline, currently
  enforced inline by each skill, no standalone linter).
- **Brand voice is team-owned** (pivot 2026-07-24): do not run `brand-voice-guard` or
  restyle for voice on community-feed drafts.
- The worth-posting gate can end in "no post." Log it and stop; never pad.
- Missing intake → blocked note, stop. Never substitute a source.
- Kill condition: anything in `drafts/community-feed/` unreviewed > 14 days in the
  first 6 weeks → revert to manual.

## Schedule

All runs are manual for now (no Actions runner; see Runtime above).

- **Weekly (Monday):** read `intake/YYYY-WW-ships.md` → run `format-weekly-shiplog` →
  draft to `drafts/community-feed/`. `format-tweet-digest` is **held** until Adrian
  picks the feed surface (2026-07-30); do not run it in the weekly loop.
- **On demand:** `format-feature-built` when a ship earns a deep-dive and someone has
  pasted its source into `intake/features/`.
- **Monthly:** `monthly-content-retro`: prune losers, promote winners to `patterns/`.
  *(Not yet written; reads the rejected/posted split in the drafts folder.)*
