---
name: format-tweet-digest
description: Use when writing the weekly tweet-digest post for Simmer's internal community feed. Use for the roundup of notable posts from the X watchlist, "what happened on crypto/prediction-market X this week," or turning a batch of captured tweets into a community post.
---

# format-tweet-digest

## Overview

Turn a week of captured tweets into a digest for the community feed. **A digest is not a link dump.** The captures are the raw material; the product is *our read on them*. If we have nothing to add to an item, the item does not belong in the post — that's true even when the tweet is big.

Core principle: **every item carries our line.** The value we add is the thing only we can add — our own numbers, our own ships, our own disagreement. A reader can find the tweets themselves; they can't find what those tweets mean for the agent they're running.

## Inputs

- **Captures:** the week's tweets from the watchlist.
- **Watchlist:** `shared-knowledge/marketing/research/x-account-audit.md` §1 (the ten audited accounts). `research/kol-outreach-pool.md` is a candidate list for *widening* the capture pull, not a source of captures — it matters when captures are being collected, not when the digest is being written.
- **Our side:** the same week's data drop (`shared-knowledge/marketing/data-drops/YYYY-WW.md`) and ship source — needed to write our line on the items that touch us.
- **Format:** `shared-knowledge/marketing/platform-guidelines.md` → *Community feed*.
- **Gate:** `shared-knowledge/marketing/content-calendar.md` → *worth-posting gate*.

> **SOURCE ASSUMPTION (unconfirmed — Alyna/Adrian to settle):** captures are assumed to
> come from a weekly pull over the §1 watchlist. No capture pipeline exists yet. Until
> it's wired, work against `test-fixtures/tweet-captures/YYYY-WW-captures.md`. Only this
> Inputs section changes when the real pipeline lands.

## The post's shape (recipe — follow in order)

1. **Line 1 = the one item that changes what a reader should do or believe this week, with our angle already in it.** Not "here's this week's digest."
2. **Each item = capture + our line.** Two parts, always, in this order: what was posted (with the link), then one to three sentences of our read — our number, our ship, our disagreement, or what a user should do differently. **An item with no line is cut, not shortened.**
3. **Three to five items.** Rank by whether the item changes a Simmer user's decisions, not by the poster's engagement. Below three surviving items, see the gate.
4. **Close with what was skipped and why** — one line. It shows the filter is running.

**Claim handling — apply per item.**

| Item type | How it goes in the post |
| --- | --- |
| Verified fact (a shipped product, a published number) | State it, link it. |
| Unverified rumor | Label it unverified **in the same sentence** and never restate it as fact downstream. Our line must address the unverified status; it may also say why the item does or doesn't matter to a user either way. |
| Technical analysis / prediction | Attribute to the account ("@X reads it as…"), never adopt as our view. Adopting someone's price call is us making a price call. |
| Competitor marketing | Include only if it tells our reader something true about the market. Relay the *fact*, never the pitch. |
| Our own numbers used as the line | Must trace to the data drop, same as any other claim. |

## Process

1. Read the captures. Read the same week's data drop and ship source — you cannot write our line without them.
2. For each capture, write the candidate line **first**. No line → cut the item. Do this before ranking; it's the filter.
3. Rank surviving items by decision-impact for a user running an agent. Keep 3–5.
4. **Worth-posting gate** (`content-calendar.md`): fewer than three items survive → log `no post — nothing cleared the gate` with the reason and stop. A quiet week on X is a real outcome.
5. Apply the claim-handling table to every item.
6. Build the post to the shape above. Format per `platform-guidelines.md` → *Community feed*. Do **not** run `brand-voice-guard`: voice is team-owned (pivot 2026-07-24).
7. Build the **claim→evidence map**: each capture maps to its link; each of our lines maps to a data-drop or ship-source line (`claim-evidence-linter`). A line claiming we *don't* have something ("we don't surface multi-leg orders yet") maps to the source you searched and the fact that nothing matched — say which list you checked. If you can't name the list, you're guessing about our own product; cut the line.
8. Emit the draft with its tier tag.

## Output schema
A Paperclip draft issue:
```
Draft: tweet digest — YYYY-WW
Surface: community feed
Tier: 2
Items kept: <n>   Items cut: <n> (<reason categories>)
Body: <post text>
Claim→evidence map:
  - <capture> → <permalink>
  - <our line> → <source file>:<line or PR/issue id>
Gate: <passed | "no post — nothing cleared the gate" + reason>
```
*Paperclip isn't wired to this repo yet — until it is, return the draft as your response rather than filing it.*

## Quality gates
- Every item has our line. **Zero bare relays.**
- 3–5 items. Not ten.
- Every capture has a working permalink.
- Unverified claims labelled inline; no TA or price call adopted as ours.
- Every one of our lines mapped to evidence; **zero unmapped claims.**
- Ranked by decision-impact, not by like count.

## Worked example (this is the "good" bar)

Input: `test-fixtures/tweet-captures/2026-W30-captures.md` + `test-fixtures/data-drops/2026-W30.md` + `test-fixtures/ship-logs/2026-W30-ships.md`.

**Weak (what an agent does WITHOUT this skill):** keeps six items and leads well on the Bankr tweet — then pads. "@Kalshi shipped multi-leg orders… flagging it because a few of you have asked," "@RektCapital — BTC retesting its 50-month EMA… noting it's in the water this week," "@aeonframework shipped ADK v0.9… worth a skim." Three relays with no read. The good item is diluted by the items that only exist to reach six.

**Good (four items):**
> **@Bankr put a number on the thing that cost us $3,970 this week.** They measured the "surprise" Fed headline moving price 33¢ before the correction landed ([link]). That's the same headline our News Chaser agents bought at 0.64 and rode to 0.31 — six agents, one source. We merged the second-source check on Thursday (#412); it goes default-on 7/31. If you run News Chaser, that's the week your agent's behavior changes.
>
> **@Polymarket says Fed decision markets crossed $40M, their biggest macro market ever** ([link]). Matches our side: macro was 48% of our volume this week. If you're running strategies tuned for sports liquidity, they're now trading a different book.
>
> **@Kalshi shipped multi-leg orders — up to four contracts in one ticket** ([link]). We don't surface these in agent config yet. Until we do, a multi-leg position is something you build by hand, and your agent won't see it as one position.
>
> **Unverified: an alpha account is claiming Polymarket is raising at $5B** ([link]). No primary source and the post says so itself. It's here because it has 6.2K likes and you'll see it repeated — treat it as a rumor, and note that a raise wouldn't change anything about how your agent trades this week.
>
> *Skipped: TA calls, competitor pitches, and GM posts — nothing we could add to.*

Why it works: four items, each carrying our number or our ship; the rumor is labelled and its inclusion is justified by our line about it; the Kalshi item admits a gap in our own product rather than relaying the feature.

## Common mistakes
- **Bare relay.** "Worth a skim if you're building." That's the tweet, not our read. Cut the item.
- **Padding to a number.** Six items because six feels like a digest. Four with lines beats six with three relays.
- **Ranking by likes.** The 6.2K-like rumor is not the lead; the 780-like tweet that touches our PnL is.
- **Adopting someone's price call.** "BTC resolves within 9 days" stated as information is us making the call.
- **Relaying a competitor's pitch.** "PMX solves this — our books are market-made from launch" is their copy. Either say what it tells our reader about thin books, or cut it.
- **Quoting our own numbers from memory.** Our line needs an evidence-map entry too.

## Failure handling
Missing captures → file a Paperclip issue, do not draft. Missing data drop/ship source → draft only the items whose lines don't depend on them; if that drops the count below three, log the gate failure. Failed gate → log and stop; never auto-clear.

## Escalation
**Tier 2 is the normal outcome** (Alyna review, < 24h) — including items that mention competitors neutrally or report our own bad week. Escalate to Tier 3 (Adrian) if an item criticises a named competitor, touches a partnership or anything commercial, or repeats a rumor about **a venue we route trades to or a named partner** (Polymarket and Kalshi both count — a rumor about a venue holding user money is Adrian's call, not a judgment call).
