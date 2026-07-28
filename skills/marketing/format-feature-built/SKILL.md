---
name: format-feature-built
description: Use when writing a "feature built" deep-dive for Simmer's internal community feed — one shipped feature, gone deep. Use when a single ship deserves more than a shiplog line, when explaining how something works and why it was built that way, or for the build-in-public writeup of a merged feature.
---

# format-feature-built

## Overview

One shipped feature, gone deep, for the community feed. The shiplog says *what* changed; this post says *how it works, what we chose against, and what it still doesn't do.*

Core principle: **the limits are the content.** Anyone can describe a feature. What a reader running real money can't get anywhere else is the trade-off we made, the alternative we rejected and why, and the case this still doesn't cover. A deep-dive that only explains the happy path is a product page.

Second principle, from the RED baseline: **the temptation in this format is to be helpful about things the source doesn't say.** Rollout mechanics, opt-outs, and the reason behind a number are exactly where invented detail creeps in — and on this feed an invented capability is one a reader will go try and fail to find.

## Inputs

- **Feature source:** the PR body, PR-thread design discussion, linked issue, and any backtest or benchmark attached.
- **Context:** the week's data drop if the feature answers something in it; the ship source for rollout state (flags, dates). **Check the dates before you connect them** — see Common mistakes.
- **Format:** `shared-knowledge/marketing/platform-guidelines.md` → *Community feed*.
- **Gate:** `shared-knowledge/marketing/content-calendar.md` → *worth-posting gate*.

> **SOURCE ASSUMPTION (unconfirmed — Alyna/Adrian to settle):** assumed to be the merged
> PR plus its thread. Until wired, work against `test-fixtures/features/*.md`. If design
> discussion lives somewhere other than the PR thread, only this section changes.

**A feature with no design discussion and no measurement is not a deep-dive candidate** — it's a shiplog line. See Failure handling.

## The post's shape (recipe — follow in order)

1. **Line 1 = what a user can now do, or the failure that forced this.** If the feature exists because something broke, the break is the lead — with its cost in it **if the source attaches a cost to that break.** No number attached → state the break without one. Do not borrow a number from the backtest to dress up line 1; the measurement is not the incident.
2. **How it works.** The actual steps, with the real names — flags, components, thresholds, log destinations. This audience wants the mechanism.
3. **The decisions.** At least one trade-off *and* at least one rejected alternative, with the reason. Quote the builder from the source where the source has their words; a paraphrase of a sharp answer is a worse answer.
4. **The receipt.** The measurement, stated with its cost. A backtest that catches 11 of 14 also produced 2 false blocks — both numbers or neither.
5. **What isn't true yet.** **Required section.** Known holes, what it is *not* wired into, and the current rollout state with its date.
6. **What you do about it** — only if the source proves an action exists. No proven action → this section is omitted entirely, not softened into a suggestion.

## Process

1. Read the feature source end to end, including the thread. Note which facts are *stated* versus which you are inferring.
2. **Worth-posting gate** (`content-calendar.md`): the feature must have a real trade-off or measurement to discuss. A clean feature with no decisions and no numbers → log `no post — nothing cleared the gate; shiplog line instead` and stop.
3. Build the post to the shape above. Sections 3, 4, and 5 are all required; a post missing any of them is not this format.
4. Format per `platform-guidelines.md` → *Community feed*. Do **not** run `brand-voice-guard`: voice is team-owned (pivot 2026-07-24).
5. **Build the claim→evidence map, then delete what isn't in it.** Every mechanism, number, date, rollout state, causal claim, and reader action maps to a source line. **A claim you believe is true but cannot map is cut** (`claim-evidence-linter`).
6. Run the reader-action check: for every sentence telling the reader they can do something, name the source line that proves the capability exists. No line → delete the sentence.
7. Emit the draft with its tier tag.

## Output schema
A Paperclip draft issue:
```
Draft: feature built — <feature> (<PR id>)
Surface: community feed
Tier: 2
Body: <post text>
Claim→evidence map:
  - <claim> → <source file>:<line or PR/issue/thread ref>
Reader actions asserted: <action> → <source line proving it exists>   (or "none")
Gate: <passed | "no post — nothing cleared the gate" + reason>
```
*Paperclip isn't wired to this repo yet — until it is, return the draft as your response rather than filing it.*

## Quality gates
- Line 1 is a capability or a failure — not "this week we shipped."
- Trade-off section present, with at least one rejected alternative and its reason.
- Receipt present, with the cost alongside the win.
- **"What isn't true yet" section present**, including rollout state and date.
- Every claim mapped; **zero unmapped claims**, including reasons-behind-numbers and rollout mechanics.
- Every asserted reader action traced to a source line.

## Worked example (this is the "good" bar)

Input: `test-fixtures/features/2026-W30-headline-verification.md`.

**Weak (what an agent does WITHOUT this skill):** genuinely strong on the mechanism, the 90-second rationale, the rejected LLM check, and the syndication hole — then invents. *"If you'd rather stay on single-source entries past the switchover, you'll be able to opt back out — details in the flip announcement."* No opt-out exists in the source. *"2 false blocks — held back because a real second source just wasn't fast enough to confirm inside the window."* The source gives the count, never the reason. Both read as helpful; both are the agent filling gaps, and a reader will go looking for an opt-out that isn't there.

**Good — the same passages, written to the source:**
> **Where it stands.** The flag is `HEADLINE_GATE`, off by default since merge, and it flips to default-on 2026-07-31. It's wired into News Chaser only — no other strategy is touched. We shipped it off by default because we didn't want to change a live strategy's behavior mid-week without warning.
>
> **The receipt.** Replayed against the 14 corrected-headline events in our logs since 2026-05-01: 11 blocked, 3 not, 2 false blocks. The 3 misses were all the syndication case — every configured source was running the same wire copy, so two-source matching agreed on the wrong claim. The 2 false blocks were entries that would have been fine. We're not going to pretend the gate is free.
>
> **What it doesn't fix.** Syndication, per above — this closes the single-source case, which is what actually happened, and nothing more.

Why it works: rollout state is stated exactly as the source states it, with no invented user control; the false-block count appears without an invented cause; the limitation is a section, not a footnote.

## Common mistakes
- **Inventing the opt-out.** The most likely failure in this format. If the source doesn't describe a user-facing control, the post doesn't either.
- **Explaining a number the source only counts.** "2 false blocks" is evidence. "2 false blocks *because the second source was slow*" is fiction.
- **Selective receipts.** Citing 11-of-14 without the 3 misses and the 2 false blocks.
- **Happy path only.** No rejected alternative, no hole → it's a product page, and this format has nothing to say.
- **Deep-diving a feature with no decisions.** Not every ship earns this post. Log the gate failure and let it be a shiplog line.
- **Paraphrasing the builder.** The thread's own words are the most credible thing in the source.
- **Stitching a false origin story.** The most seductive version of the fabrication failure: a loss in this week's data drop looks like the reason the feature exists, so the post says it caused the work — when the linked issue was filed *before* it. Compare the issue's open date to the incident's date before writing any "because." If the issue came first, the incident is at most a demonstration of the gap, not its origin, and the post must say which.

## Failure handling
Feature source thin (no design discussion, no measurement) → log the gate failure and route the ship to `format-weekly-shiplog` instead. Backtest referenced but not attached → state the claim as unmeasured or cut it; never quote a number you can't map. Failed gate → log and stop; never auto-clear.

## Escalation
**Tier 2 is the normal outcome** (Alyna review, < 24h). Deep-diving a risk control, quoting a backtest with its false positives, or repeating a flag-flip date already fixed in the repo are all ordinary content for this format and stay at Tier 2 — escalating them all would make Tier 3 the default and the tier meaningless. Escalate to Tier 3 (Adrian) only when the post itself creates exposure the source doesn't already carry: disclosing a limitation not written down anywhere yet, announcing a default trading-behavior change ahead of the team, or committing to a date the source doesn't state.
