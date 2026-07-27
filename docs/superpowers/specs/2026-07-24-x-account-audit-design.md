# Research brief — X-account audit

*Author: Alyna · Date: 2026-07-24 · Status: Approved for execution*

---

## Why this exists

Direction update from Adrian: start simpler than the full X agent — an internal
community feed first, then expand to X. The three starting formats are already
chosen on our end (weekly **shiplog**, **tweet digest**, **"feature built"**
deep-dive), borrowed from how the Aeon project runs its community feed (Aeon's
"Agent Feed" TG group, `@aeon_agent`). Brand-voice matching is now owned by the
team, not the agent.

The highest-leverage thing to do next plays to research strength: **the same move
we made studying the Aeon feed, one level wider and on X.** Audit the best
accounts in our space and bring back what they do — a map of who's doing X well,
and our next set of format ideas, *before* we start posting there ourselves.

## Deliverables

1. **Shortlist** of the best profiles to model — each with a one-line "why it's good."
2. **Content-type breakdown** for the strong ones — what they post, how often,
   which posts get real engagement, and what we could borrow.
3. **"What we'd borrow"** synthesis — ranked, repeatable patterns, each mapped to
   one of our three community formats (shiplog / tweet-digest / feature-built) or
   flagged as a new X-format idea.

**Hard rule: real material only.** Screenshots or links to actual posts with real
engagement numbers — never summaries from memory.

## Scope

~12–16 accounts across four categories:

1. Prediction-market platforms
2. Trading-bot builders
3. AI-agent projects
4. Public-wallet traders / KOLs (individual personalities posting real results)

Seed accounts (named by Adrian): `@aeon_agent` (Aeon), pmxt, FunctionSpace.
Discover the rest **live** — who the seeds engage with, prediction-market CT, and
the KOL outreach pool below.

Deep breakdown on the **~6 strongest**; one-line "why" on the rest.

### KOL / public-wallet candidate pool

Category-4 seed pool (accounts the team has already reached out to). Audit a
**subset** — pull the ~4–6 strongest (real results + real engagement on X) into
the shortlist; log the rest here as pool.

Clear handles (findable live): rektcapital, sairahul1, mert, luishXYZ, cvxv666,
0xMovez, PredMTrader, dkposts, Rifat_EE, thenarrator, AIRDROPS.IO, Blockchain
Surfer, Oracle Boar, Predict0r, Recogard, VALIX, Punisher, DerParsel, Spivach,
ArchiveExplorer, AlterEgo, Rhino McCoy, Jon Becker, Bizzi (Chaos), 0x Discover,
AdiiX, Philanthrop, Mahera, Lutchyn, Lunar, Ronin, Hector.

Ambiguous / first-name-only (skip until handles provided): Jayden, Roan, Vlad,
Scott, Jonze, Kober, Drew, Nitesh, Marko, tsybka, ih8y, evans, Aleiah.

## Per-account schema

**All accounts (shortlist row):** handle + link · category · rough size ·
one-line "why it's good."

**~6 strongest (deep breakdown):**
- Content types posted, with rough % mix
- Cadence (how often they post)
- **2–3 top-performing posts** each — screenshot + link + real engagement
  (views / likes / reposts), timestamped at capture
- "What we'd borrow" line

## Method

- Live via the gstack `/browse` skill against the operator's logged-in Chrome.
- Visit each profile + its top posts; screenshot real ones into
  `shared-knowledge/marketing/research/assets/`; record links + numbers.
- X engagement is a moving snapshot — every capture is timestamped.
- If X isn't authenticated, stop and flag rather than collect thin data.

## Output

Single living doc: `shared-knowledge/marketing/research/x-account-audit.md`

1. How to read + method / capture date / caveats
2. Shortlist table (all 12–16) — deliverable 1
3. Deep breakdowns (~6 strongest, each linked from the table) — deliverable 2
4. "What we'd borrow" — synthesis mapped to our three formats or flagged new

Lives in `shared-knowledge/` so future skills can read it. Screenshots alongside
in `research/assets/`.

## Repo housekeeping (the pivot)

Reversible edits recording the direction change:
- **Pause `format-receipts-post`** — status `Paused (Adrian, 2026-07-24 —
  community-feed-first pivot)` in `skills/marketing/SKILLS.md` + a one-line banner
  atop `format-receipts-post/SKILL.md`. No deletion.
- **Record the direction** in `docs/specs/2026-07-24-community-feed-pivot.md`:
  community feed first → then X; the three chosen formats; brand-voice now
  team-owned.
- **Save the KOL outreach pool** to
  `shared-knowledge/marketing/research/kol-outreach-pool.md`.

## Out of scope

- Matching brand voice (team-owned now).
- Building the community-feed formats themselves (chosen on their end).
- Auditing all ~40 KOLs deeply — subset only.
- Live posting.
