# Agent: marketing-agent

*Extends Aeon. Not a new runtime, not a new agent per channel.*

| Field | Value |
| --- | --- |
| Role | Marketing — turn trading data + intel into publishable content |
| Runtime | Claude Code on GitHub Actions, scheduled (same as Aeon) |
| Model | Sonnet 4.6 (July 2026 snapshot; retune as cost/capability shift) |
| Coordination | Paperclip only. A content idea is an issue, never a chat. |
| Knowledge | reads/writes `shared-knowledge/marketing/`; reads `intel/`, `patterns/` |

## Tool access (matrix, not free-for-all)

| Tool | Access | Notes |
| --- | --- | --- |
| QMD (wiki search) | read | Sense stage runs over wiki + intel |
| Weekly trade artifact | read | Source for the data drop |
| Paperclip | read/write | Files opportunity + draft issues |
| X / LinkedIn (MCP) | **draft-only, weeks 1–6** | Tier-gated; no autonomous posting during MVP |
| UTM/link tooling | write | Tags published links for attribution |

## Operating rules

- Add skills, not agents. Every format is a skill file.
- No claim ships without an evidence map (`claim-evidence-linter`).
- Strategy before volume: pause before inventing positioning or fanning one
  idea into ten shallow posts.
- Respect the publish tiers (see PRD §5). Kill condition: Tier-2 queue > 14 days
  in the first 6 weeks → revert to manual.

## Schedule (proposed)

- **Weekly:** build data drop → sense opportunities → draft receipts post → route to tier.
- **Monthly:** `monthly-content-retro` — prune losers, promote winners to `patterns/`.
