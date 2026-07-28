---
name: format-weekly-shiplog
description: Use when writing the weekly "what we shipped" post for Simmer's internal community feed. Use for the weekly ship roundup, the Friday/Monday shiplog slot, turning a week of merged PRs and closed issues into a community post, or "what did we ship this week."
---

# format-weekly-shiplog

## Overview

Turn one week of repo activity into a shiplog post for the community feed. **A shiplog is not a changelog.** A changelog lists what moved in the repo; a shiplog tells users whose money is on the line what changed for them and what it was worth.

Core principle: **ships alone flatline.** Aeon runs a pure ship-as-post feed to 150.1K followers and lands 2.1K–7.4K views a post — 1.4–5% of its following (`research/x-account-audit.md` §2, §3 caution 2). The thing that makes our version worth reading is the layer Aeon doesn't have: **the week's real outcome sitting next to the week's ships.** Pulling the data drop is mandatory, not a nice-to-have.

## Inputs

- **Ship source:** the week's merged PRs, closed Paperclip issues, and open/unmerged branches.
- **Outcome source:** the same week's data drop (`shared-knowledge/marketing/data-drops/YYYY-WW.md`).
- **Anchor detail:** the anchor ship's PR body and thread, if the ship source only gives you a title. A one-line PR summary will not carry the anchor paragraph — go read the PR.
- **Format:** `shared-knowledge/marketing/platform-guidelines.md` → *Community feed*.
- **Gate:** `shared-knowledge/marketing/content-calendar.md` → *worth-posting gate*.

> **SOURCE ASSUMPTION (unconfirmed — Alyna/Adrian to settle):** the ship source is
> assumed to be merged PRs + closed Paperclip issues for the window. Until it's wired,
> work against `test-fixtures/ship-logs/YYYY-WW-ships.md`. If the real source turns out
> to be a changelog or release notes, only this Inputs section changes — the recipe holds.

Missing data drop → see Failure handling. Missing ship source → do not draft.

## The post's shape (recipe — follow in order)

1. **Line 1 = the week's outcome or the one ship that changes what a user can do.** Never "here's what we shipped this week." The reader learns in one line whether this was a good week and what it cost or bought them.
2. **The receipts beat.** The week's real result from the data drop — net, win rate, worst strategy, losses in. If a ship exists *because* of something in the drop, say so here; that link is the whole reason this post beats a changelog.
3. **The anchor ship** — one ship gets a real paragraph: what changed, what a user can now do, and its current rollout state (flag, default, date).
4. **The rest — at most four, one line each**, each written as a user-facing change ("you can now …"), never as a PR title. PR/issue IDs in parentheses.
5. **What's changing next** — only dates already fixed in the source (a scheduled flag flip is a fact). Nothing else.

**Ship-selection filter — apply before writing.** Keep a ship only if a user's experience changes. Cut dependency bumps, refactors with no behavior change, CI and test fixes, and internal tooling. **Cut everything not merged** — draft PRs, spikes, and design-only work are not ships and do not appear in the post, not even as "in progress." (`content-calendar.md`: roadmap teases don't count.)

**If more than five ships survive the filter**, rank by how much of a user's behavior changes: (1) changes what a live agent does by default, (2) removes a limit users have hit — an issue with upvotes or linked support tickets is evidence of this, (3) new capability nobody asked for yet, (4) fixes. Anchor the top one, take the next four, drop the rest. Dropped ships are not a backlog for next week — they're dropped.

## Process

1. Read the week's ship source; apply the ship-selection filter. Count what survives.
2. Read the same week's data drop. **If you did not open the data drop, the post is not finished** — the receipts beat is required.
3. **Worth-posting gate** (`content-calendar.md`, all four criteria). Two checks, both must pass:
   - **Something changed:** at least one surviving ship changes what a user can do.
   - **Not a repeat (criterion 4):** that ship hasn't already been told to this feed. **A change we pre-announced now taking effect is a repeat.** A flag flipping on the date last week's shiplog gave is a one-line reminder inside a post that has other news — it cannot carry a post by itself.

   Either check fails → log `no post — nothing cleared the gate` with the reason and stop. A quiet week is a normal outcome; never pad with refactors, and never build a post around the fact that a previously-announced date arrived.
4. Build the post to the shape above. One anchor + ≤4 lines is the ceiling.
5. Format per `platform-guidelines.md` → *Community feed*. Do **not** run `brand-voice-guard`: voice is team-owned (pivot 2026-07-24). Do **not** draft a founder variant — that rule in `content-calendar.md` is scoped to public receipts posts, not the community feed.
6. Build the **claim→evidence map**: every number, date, rollout state, and causal link maps to a source line. Unmapped → cut the claim or cut the sentence (`claim-evidence-linter`).
7. Emit the draft with its tier tag.

## Output schema
A Paperclip draft issue:
```
Draft: weekly shiplog — YYYY-WW
Surface: community feed
Tier: 2
Ships kept: <n>   Ships cut: <n> (<reason categories>)
Body: <post text>
Claim→evidence map:
  - <claim> → <source file>:<line or PR/issue id>
Gate: <passed | "no post — nothing cleared the gate" + reason>
```
*Paperclip isn't wired to this repo yet — until it is, return the draft as your response rather than filing it.*

## Quality gates
- Line 1 is an outcome or a user-facing change — not "this week we shipped."
- The receipts beat is present and traces to the week's data drop.
- Every item in the body is merged. Zero unmerged/in-design items.
- One anchor ship + at most four one-liners.
- Every claim mapped to evidence; **zero unmapped claims.** Causal and forward claims count ("this would have caught X," "you'll be able to Y").
- Losses and limits stated straight, not softened.

## Worked example (this is the "good" bar)

Input: `test-fixtures/ship-logs/2026-W30-ships.md` + `test-fixtures/data-drops/2026-W30.md`.

**Weak (what an agent does WITHOUT this skill):** correctly cuts the refactor and the dependabot bump, but then reports the in-design portfolio view as "in progress," and asserts the headline gate "would have caught 11 of 14 **including this week's**" — the backtest never says this week's event was in the sample. One invented claim in an otherwise honest post is the whole failure.

**Good:**
> Down $2,880 this week across 41 agents, 47% win rate. Almost all of it is one strategy: News Chaser lost $3,970 buying a Fed market at 0.64 on a headline that got corrected, then rode it to 0.31. Six agents, one source, no check.
>
> **We shipped the check (#412).** News Chaser now has to find the same claim in a second independent feed within 90 seconds before it can enter. No match, no entry — and it writes down why it stood down. Backtested against the 14 corrected-headline events in our logs since May: it blocks 11 of them, misses 3 where every source ran the same wire copy, and would have blocked 2 entries that were fine. It's behind the `HEADLINE_GATE` flag, off today, default-on 7/31.
>
> Also merged:
> - You can now replay a strategy against real recorded prices before it touches money (#409 — the issue had 14 👍 on it).
> - "if BTC drops 5% then buy the recovery market" now parses instead of erroring (#405).
> - Kalshi markets show order-book depth, not just top-of-book (#411).
> - The "your first agent" page is rewritten — it had four support tickets against it (#416).

Why it works: the loss is the lead, the anchor ship is the answer to it, every number traces to a source line, the gate's cost (2 false blocks) is in, and nothing unmerged appears. Note what the good draft *doesn't* say about #409: the source gives an upvote count, so the post gives an upvote count — not "most-requested," which is a comparison the source never makes.

## Common mistakes
- **Changelog with no outcome.** A list of ships and no data drop. This is the Aeon failure mode — see Overview.
- **Inventing the connective tissue.** "This would have caught this week's blowup," "every other strategy was flat" — plausible, unsourced, cut.
- **Roadmap creep.** "In progress," "coming soon," "more once there's something to look at." Not shipped, not in the post.
- **PR titles as bullets.** "Surface Kalshi order-book depth in market view" → "Kalshi markets now show order-book depth."
- **Padding a quiet week.** Four internal refactors dressed up as ships. Log the gate failure instead.
- **Reporting the calendar.** "The flag we announced last week flipped on schedule" is not news, it's the absence of news. If it's the only thing you have, you don't have a post.

## Failure handling
Missing data drop → do not publish a ships-only post; draft it, annotate `blocked: no data drop for YYYY-WW`, route for human review. Missing/empty ship source → file a Paperclip issue, do not draft. Failed gate → log the gate line and stop; never auto-clear.

## Escalation
**Tier 2 is the normal outcome** (Alyna review, < 24h) — a shiplog reporting merged work on the internal feed stays at Tier 2 even when it reports a loss, quotes a backtest, or names a date already fixed in the repo. Escalate to Tier 3 (Adrian) only when the post itself carries new risk: a breaking change, a default trading-behavior change not yet announced anywhere, or a commitment the source doesn't already contain.
