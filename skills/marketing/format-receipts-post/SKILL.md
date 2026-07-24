---
name: format-receipts-post
description: Use when a weekly Simmer data drop exists and it's time to turn real trading results — aggregate PnL, win/loss mix, a notable failure — into an X or LinkedIn receipts post. Use when publishing proof-of-results content, the weekly receipts slot, or "what our agents actually made and lost this week."
---

# format-receipts-post

## Overview

Turn one weekly data drop into a receipts post: real results, losses included, written to stop the scroll. **The whole point is that only Simmer can write this** — aggregate PnL across many agents with the losses in. A receipts post that reads like a company changelog has failed, even if every fact is true.

Core principle: **lead with the hook, not the brand.** The strongest, hardest-to-fake thing in the drop goes in line 1. The company name, the win-rate roundup, and the CTA come later.

## The post's shape (recipe — follow in order)

1. **Line 1 = the hook.** Pick the single most scroll-stopping thing in the drop and open with it, using one hook pattern from `brand-voice.md`:
   - *the honest part* — a real loss/blowup, stated plainly
   - *the receipt* — the hardest-to-fake number, losses in
   - *the contrarian edge* — where the agents went against the crowd
   The vivid failure usually out-hooks the net number. Lead with whichever a reader would forward.
2. **Proof beats.** 2–4 real numbers that back the hook. Net result, win/loss, best/worst strategy — but only the ones that serve this post's one angle.
3. **The lesson / what changed.** What happened → why → what changes next week.
4. **One CTA, last.** A single tagged link. Plain-English outcome ("describe a strategy, your agent runs it"), never a feature name.

## Process

1. **Pick exactly ONE audience** from `audiences.md` (trader / builder / founder / partner) before writing a word. Mixed-audience drafts are rejected. If the angle is unclear, ask in the opportunity issue — don't guess and drift.
2. Build the post to the shape above. Lead with the hook; **never bury the best moment in paragraph 4.**
3. Apply `brand-voice-guard` against `brand-voice.md`: kill generic-hype openers ("Week in the books," "Get excited"), translate jargon, keep the honesty line *in* the post.
4. Format for the platform via `platform-guidelines.md` (X thread vs. LinkedIn narrative).
5. Build the **claim→evidence map**: every assertion maps to a data-drop line. Unmapped claim → cannot ship (`claim-evidence-linter`).
6. **Worth-posting gate** (`content-calendar.md`): if the week is thin or this says nothing new, log "no post — nothing cleared the gate" and stop. Never pad a boring week.
7. Draft the **founder-voiced variant** from the same evidence map (first-person, for Adrian's account; his call whether to use it).
8. Emit both drafts with the tier tag (receipts default Tier 2).

## Output schema
A Paperclip draft issue:
```
Draft: receipts post — YYYY-WW
Audience: <one>
Tier: 2
Hook type: <honest-part | receipt | contrarian-edge>
Body (brand): <post text>
Body (founder variant): <first-person version for Adrian's account>
Claim→evidence map:
  - <claim> → data-drops/YYYY-WW.md:<line>
UTM: <tagged link from utm-tagging-and-ledger>
Gate: <passed | "no post — nothing cleared the gate" + reason>
```

## Quality gates
- Line 1 is a hook (loss, number, or edge) — NOT the company name or a hype phrase.
- Exactly one target audience.
- Every claim mapped to evidence; zero unmapped claims.
- At least one loss/failure included (credibility over polish).
- Passes `brand-voice-guard` (no overclaim, caveats present, no jargon for non-builder audiences).

## Worked example (this is the "good" bar)

Input: data drop with net +$4,120, 61% win rate, "Favorite Chaser" −$2,610 after 3 agents rode Brazil −1.5 from 0.71 to 0.38 with no stop.

**Weak (what an agent does WITHOUT this skill):** opens "Week in the books at Simmer 📈", roundup of stats, buries the Brazil blowup in paragraph 4, no single audience.

**Good (trader audience, honest-part hook):**
> Three of our agents bought Brazil −1.5 at 71¢ minutes before Japan equalized. No stop. Rode it to 38¢. 🧵
>
> That strategy ("Favorite Chaser") lost $2,610 this week. We're showing you anyway.
>
> Net across all 38 agents: still +$4,120 on a 61% win rate — because "Shock Ladder," which fades panic moves, made $3,940 doing the opposite.
>
> The lesson the agents are enforcing next week: momentum-buying favorites with no exit is how you give back a good week.
>
> Describe a strategy in plain English, watch your agent run it → [link]

Why it works: leads with the vivid loss (nobody scrolls past a blowup), the honesty *is* the hook, every number traces to the drop, one audience, one CTA.

## Common mistakes
- **Brand-first opener.** "Week in the books at Simmer" → cut it; open on the loss or the number.
- **Burying the best moment.** If the most forwardable line isn't line 1, reorder.
- **Roundup with no angle.** Dumping every stat serves no one. Pick one angle, use only the numbers that back it.
- **Two audiences.** "For traders and builders" → split into two re-argued posts.
- **Losing the honesty.** "You can still lose / it trades real money" belongs in the post, not omitted for polish.

## Failure handling
Missing data drop → do not draft; wait or file a blocked issue. Failed gate → keep as draft, annotate the failing gate, route for human review; never auto-clear.

## Escalation
Default Tier 2 (Alyna review, < 24h). Escalate to Tier 3 (Adrian) if the post touches partnerships, price, legal, or a forward-looking/strategic claim.
