# SKILL: format-receipts-post

*Phase 1. The core factual post built from the weekly data drop. Low-risk,
factual, and the first format to prove the whole loop end to end.*

## Trigger
After `weekly-data-drop-builder` emits a data drop for the week, and an
opportunity issue exists (or the weekly cadence fires). One receipts post per drop.

## Inputs
- `shared-knowledge/marketing/data-drops/YYYY-WW.md` (+ JSON sidecar).
- Brand guardrails via `brand-voice-guard` (reads `shared-knowledge/marketing/brand-voice.md`).
- `shared-knowledge/marketing/audiences.md` (for the one-audience step).
- `shared-knowledge/marketing/platform-guidelines.md` (for platform-native formatting).
- The opportunity issue for the week (audience + angle), if one exists.

## Process checklist
1. **Pause — pick one audience** (trader / builder / founder / partner) from `audiences.md` before drafting.
   If the angle is unclear, ask via the opportunity issue; do not guess and drift.
2. Draft the post around three beats: **what happened / why / what changed next week.**
3. Lead with the most credible, hardest-to-fake number from the drop — including a loss.
4. Build the **claim→evidence map**: every assertion maps to a data-drop line.
   Unmapped claim → cannot ship (`claim-evidence-linter`).
5. Apply `brand-voice-guard`: tone, banned phrases, no overclaiming, required caveats.
6. Emit the draft with its tier tag (see `publish-tier-router`; receipts default Tier 2).

## Output schema
A Paperclip draft issue containing:
```
Draft: receipts post — YYYY-WW
Audience: <one>
Tier: 2
Body: <post text>
Claim→evidence map:
  - <claim> → data-drops/YYYY-WW.md:<line>
UTM: <tagged link from utm-tagging-and-ledger>
```

## Quality gates
- Exactly one target audience.
- Every claim mapped to evidence; zero unmapped claims.
- At least one loss/failure included (credibility over polish).
- Passes `brand-voice-guard` (no overclaim, caveats present).

## Failure handling
Missing data drop → do not draft; wait or file a blocked issue. Failed gate →
keep as draft, annotate the failing gate, route for human review; never auto-clear.

## Escalation
Default Tier 2 (Alyna review, < 24h). Escalate to Tier 3 (Adrian) if the post
touches partnerships, price, legal, or a forward-looking/strategic claim.

## Make-quality notes  *(Alyna's mandate — to expand)*
What separates a shared receipts post from a changelog. Best captured as a
before/after on one real drop. *[To fill in.]*
