---
name: format-weekly-shiplog
description: Use when writing the weekly "what we shipped" post for Simmer's internal community feed. Use for the weekly ship roundup, the Monday shiplog slot, turning a week of merged PRs and closed issues into a community post, or "what did we ship this week."
---

# format-weekly-shiplog

## Overview

Turn one week of repo activity into a shiplog post for the community feed. **A shiplog is not a changelog.** A changelog lists what moved in the repo; a shiplog tells users whose money is on the line what changed for them.

Core principle: **ships only** (Adrian, 2026-07-30). Each agent's performance record is private to its owner, so the post carries no PnL line and never opens with the week's numbers. When there's a number worth showing, the receipts beat comes back; that call is Adrian's. Known trade-off: Aeon runs a pure ship-as-post feed to 150.1K followers and lands 2.1K–7.4K views a post, 1.4–5% of its following (`research/x-account-audit.md` §2, §3 caution 2). We accept that risk rather than publish numbers we've decided to keep private.

## Inputs

- **Ship source:** the week's merged PRs, closed Paperclip issues, and open/unmerged branches.
- **Anchor detail:** the anchor ship's PR body and thread, if the ship source only gives you a title. A one-line PR summary will not carry the anchor paragraph; go read the PR.
- **Format:** `shared-knowledge/marketing/platform-guidelines.md` → *Community feed*.
- **Gate:** `shared-knowledge/marketing/content-calendar.md` → *worth-posting gate*.

> **Source (v1, manual intake):** `shared-knowledge/marketing/intake/YYYY-WW-ships.md`,
> pasted weekly by the team (`intake/README.md`). When an automated source is wired
> (merged PRs, a changelog), only this Inputs section changes; the recipe holds.

Missing intake file → do not draft; write a blocked note (see Failure handling).

## The post's shape (recipe: follow in order)

1. **Line 1 = the one ship that most changes what a user can do.** Never "here's what we shipped this week." The reader learns in one line what they can now do that they couldn't last week.
2. **The anchor ship**: one ship gets a real paragraph: what changed, what a user can now do, and its current rollout state (flag, default, date).
3. **The rest: at most four, one line each**, each written as a user-facing change ("you can now …"), never as a PR title. PR/issue IDs in parentheses.
4. **What's changing next**: only dates already fixed in the source (a scheduled flag flip is a fact). Nothing else.

**Ship-selection filter (apply before writing).** Keep a ship only if a user's experience changes. Cut dependency bumps, refactors with no behavior change, CI and test fixes, and internal tooling. **Cut everything not merged**: draft PRs, spikes, and design-only work are not ships and do not appear in the post, not even as "in progress." (`content-calendar.md`: roadmap teases don't count.)

**Announceable is a separate test from merged, and it is a human call.** A real, working, merged ship can still be wrong to announce: groundwork for an integration users can't use end to end yet (announcing it reads as "it's live" when it isn't), or work on a surface the team hasn't decided to launch (a broadcast post *is* a launch). No evidence map catches this; the source can't tell you. If a ship might be in this category, keep it in the draft but mark its line `announce?` for the reviewer. Never resolve that question yourself, in either direction.

**If more than five ships survive the filter**, rank by how much of a user's behavior changes: (1) changes what a live agent does by default, (2) removes a limit users have hit (an issue with upvotes or linked support tickets is evidence of this), (3) new capability nobody asked for yet, (4) fixes. Anchor the top one, take the next four, drop the rest. Dropped ships are not a backlog for next week; they're dropped.

## Process

1. Read the week's ship source; apply the ship-selection filter. Count what survives.
2. **Worth-posting gate** (`content-calendar.md`, all four criteria). Two checks, both must pass:
   - **Something changed:** at least one surviving ship changes what a user can do.
   - **Not a repeat (criterion 4):** that ship hasn't already been told to this feed. **A change we pre-announced now taking effect is a repeat.** A flag flipping on the date last week's shiplog gave is a one-line reminder inside a post that has other news; it cannot carry a post by itself.

   Either check fails → log `no post: nothing cleared the gate` with the reason and stop. A quiet week is a normal outcome; never pad with refactors, and never build a post around the fact that a previously-announced date arrived.
3. Build the post to the shape above. One anchor + ≤4 lines is the ceiling.
4. Format per `platform-guidelines.md` → *Community feed*. Do **not** run `brand-voice-guard`: voice is team-owned (pivot 2026-07-24). Do **not** draft a founder variant; that rule in `content-calendar.md` is scoped to public receipts posts, not the community feed.
5. Build the **claim→evidence map**: every number, date, rollout state, and causal link maps to a source line. Unmapped → cut the claim or cut the sentence (`claim-evidence-linter`).
6. Emit the draft with its tier tag.

## Output schema
A Paperclip draft issue:
```
Draft: weekly shiplog, YYYY-WW
Surface: community feed
Tier: 2
Ships kept: <n>   Ships cut: <n> (<reason categories>)
Announce flags: <ship lines marked announce? | none>
Body: <post text>
Claim→evidence map:
  - <claim> → <source file>:<line or PR/issue id>
Gate: <passed | "no post: nothing cleared the gate" + reason>
```
*Paperclip isn't wired yet: write the draft (full schema, not just the body) to `drafts/community-feed/YYYY-WW-shiplog.md`; that folder is the review queue (see its README).*

## Quality gates
- Line 1 is a user-facing change, not "this week we shipped."
- No performance numbers. Per-agent PnL is private to its owner (2026-07-30); the post carries no PnL line.
- Every item in the body is merged. Zero unmerged/in-design items.
- Any ship that might not be announceable carries an `announce?` flag for the reviewer.
- One anchor ship + at most four one-liners.
- Every claim mapped to evidence; **zero unmapped claims.** Causal and forward claims count ("this would have caught X," "you'll be able to Y").
- Limits and costs stated straight, not softened.

## Worked example (this is the "good" bar)

Input: `test-fixtures/ship-logs/2026-W30-ships.md`.

**Weak (what an agent does WITHOUT this skill):** correctly cuts the refactor and the dependabot bump, but then reports the in-design portfolio view as "in progress," and asserts the headline gate "would have caught 11 of 14 **including this week's**" when the backtest never says this week's event was in the sample. One invented claim in an otherwise honest post is the whole failure.

**Good:**
> **News Chaser agents can no longer enter on a single unverified headline (#412).** An agent now has to find the same claim in a second independent feed within 90 seconds before it can enter. No match, no entry, and it writes down why it stood down. Backtested against the 14 corrected-headline events in our logs since May: it blocks 11 of them, misses 3 where every source ran the same wire copy, and would have blocked 2 entries that were fine. It's behind the `HEADLINE_GATE` flag, off today, default-on 7/31.
>
> Also merged:
> - You can now replay a strategy against real recorded prices before it touches money (#409; the issue had 14 👍 on it).
> - "if BTC drops 5% then buy the recovery market" now parses instead of erroring (#405).
> - Kalshi markets show order-book depth, not just top-of-book (#411).
> - The "your first agent" page is rewritten; it had four support tickets against it (#416).

Why it works: line 1 is the user-facing change, the gate's cost (2 false blocks) is in, every number traces to a source line, and nothing unmerged appears. Note what the good draft *doesn't* say about #409: the source gives an upvote count, so the post gives an upvote count, not "most-requested," which is a comparison the source never makes.

## Common mistakes
- **Leading with numbers we keep private.** Opening with the week's PnL or win rate. Per-agent performance is private to its owner; ships only until Adrian decides a number is worth showing.
- **Announcing a merged ship that isn't announceable.** Plumbing for an integration users can't use yet, or work on an unlaunched surface, gets an `announce?` flag for the reviewer, not a bullet stated as live.
- **Inventing the connective tissue.** "This would have caught this week's blowup," "every other strategy was flat": plausible, unsourced, cut.
- **Roadmap creep.** "In progress," "coming soon," "more once there's something to look at." Not shipped, not in the post.
- **PR titles as bullets.** "Surface Kalshi order-book depth in market view" → "Kalshi markets now show order-book depth."
- **Padding a quiet week.** Four internal refactors dressed up as ships. Log the gate failure instead.
- **Reporting the calendar.** "The flag we announced last week flipped on schedule" is not news, it's the absence of news. If it's the only thing you have, you don't have a post.

## Failure handling
Missing/empty intake file → do not draft; write `Blocked: no ship intake for YYYY-WW` to `drafts/community-feed/YYYY-WW-shiplog.md` and stop; never scrape a substitute source. Failed gate → log the gate line and stop; never auto-clear.

## Escalation
**Tier 2 is the normal outcome** (Alyna review, < 24h): a shiplog reporting merged work on the internal feed stays at Tier 2 even when it quotes a backtest or names a date already fixed in the repo. `announce?` flags are resolved by the reviewer, never by the agent. Escalate to Tier 3 (Adrian) only when the post itself carries new risk: a breaking change, a default trading-behavior change not yet announced anywhere, or a commitment the source doesn't already contain.
