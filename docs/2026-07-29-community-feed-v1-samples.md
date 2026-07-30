# Community feed v1: sample posts for review

*Author: Alyna · 2026-07-29 · For: Adrian · Marked up with Adrian's decisions 2026-07-30*

One finished post per format, produced by the actual skills against the synthetic
2026-W30 fixtures (**none of these numbers are real**; the point is the shape, not the
content). React to these, not to the skill files: each post below *is* the format spec,
executed. The knobs you can turn are at the bottom, with the 2026-07-30 decisions
recorded inline.

How they were built and tested: `test-fixtures/README.md` (RED/GREEN method: a fresh
agent without the skill invented facts in all three formats; the skills exist to make
that impossible). Full pipeline: `intake/` → skills → `drafts/community-feed/` → human
paste. Draft-only throughout.

---

## 1 · Weekly shiplog (`format-weekly-shiplog`)

**The format (as decided 2026-07-30): ships only.** Per-agent PnL is private to its
owner, so the post carries no PnL line; the receipts beat comes back when there's a
number worth showing (Adrian's call). The original sample led with the week's PnL;
that opener is cut here to match. Known trade-off: Aeon posts pure ship-as-post to
150.1K followers and reaches 1.4–5% of them, and we accept that rather than publish
private numbers.

> **News Chaser agents can no longer enter on a single unverified headline (#412).** An agent now needs a second independent source confirming the same claim within 90 seconds before it can enter. No match, no entry, and the reason gets written to the agent's log. Backtested against the 14 corrected-headline events in our logs since May: it blocks 11 of them, misses 3 where every source ran the same wire copy, and would have blocked 2 entries that were actually fine. It's behind the `HEADLINE_GATE` flag, off today, default-on 2026-07-31.
>
> Also merged:
> - You can now replay a strategy against real recorded market prices before it touches money (#409; closed the issue on exactly this, 14 👍).
> - "if BTC drops 5% then buy the recovery market" now parses instead of throwing an error (#405).
> - Kalshi markets now show order-book depth, not just top-of-book (#411).
> - The "your first agent" page is rewritten; it had four linked support tickets against it (#416).

*Under the hood: 5 ships kept, 5 cut (refactor, dep bumps, CI fix, two unmerged). Every
number has an evidence-map entry. On a quiet week this format outputs "no post:
nothing cleared the gate" instead. Tested, and confirmed on the first real week
(2026-07-30): three commits, two of them internal plumbing, no post. A merged ship can
also be real but not announceable yet; the skill flags those `announce?` for the
reviewer, it never decides.*

## 2 · Tweet digest (`format-tweet-digest`)

**The bet:** a digest is our *read*, not a link dump; every item carries a line only we
can write (our number, our ship, our gap). An item we can't add to gets cut, even a big
one. 3–5 items, ranked by user impact, not likes.

> **@Bankr put a number on the thing that cost us $3,970 this week.** They measured the "surprise" Fed headline moving price 33¢ before the correction landed ([link]). That's the same headline our News Chaser agents bought at 0.64 on and rode down to 0.31: six agents, one uncorroborated source, no verification step. We merged the fix Thursday (#412, headline-verification gate); it's flagged off today and flips to default-on 2026-07-31. If you run News Chaser, that's the week your agent's behavior changes.
>
> **@Polymarket says Fed decision markets crossed $40M in volume, their biggest macro market yet** ([link]). That matches our own week: macro was 48% of our $96k notional traded. If your agent is tuned for sports-book liquidity, it's trading a different book than usual right now.
>
> **@Kalshi shipped multi-leg orders, up to four contracts in one ticket** ([link]). We don't surface multi-leg positions in agent config yet; until we build it, a multi-leg position is something you assemble by hand, and your agent won't see it as one position.
>
> **Unverified: an alpha account is claiming Polymarket is raising at a $5B valuation** ([link]). No primary source, and the post says so itself. It's here because it has 6.2K likes and you'll see it repeated; treat it as a rumor. Even if true, it wouldn't change anything about how your agent trades this week.
>
> *Skipped: a BTC TA call, a competitor's pitch, GM posts, and a market-hours reminder. Nothing we could add a line to.*

*Under the hood: 4 kept, 6 cut. The rumor item auto-escalated to Tier 3 (your review)
because it repeats a rumor about a venue holding user money. On a quiet watchlist week
this outputs "no post". Tested.*

## 3 · "Feature built" deep-dive (`format-feature-built`)

**The bet:** the limits are the content. The deep-dive must carry a trade-off, a rejected
alternative, the measurement *with its cost*, and a required "what isn't true yet", or
the skill refuses and routes it back to a shiplog line (tested on a thin feature).

> **What changed.** News Chaser used to let an agent enter on a single headline claim. As of PR #412 (adrian, merged 2026-07-24, closes FIXT-231 "Agents enter on unverified headlines"), a headline claim needs a second independent source to agree before entry is allowed.
>
> **How it works.** A headline comes in off the news feed and `ClaimExtractor` reduces it to a single claim sentence. `SourceMatcher` then checks the other configured feeds for that same claim inside a 90-second window. Match → the entry proceeds. No match → the entry is blocked, and the reason is written to `agent.log`. If sources conflict outright, the agent stands down and the headline goes into its do-not-retry set.
>
> **The decisions.** On the 90-second window, adrian: "longer window catches more corrections but the edge on a real headline is mostly gone by ~2min. 90 is where we still catch most corrections without giving up all the edge." It's tunable, not derived; expected to move.
>
> The team also considered asking a model whether a headline looked true, and rejected it: "Non-deterministic, can't audit why it fired, and in the spike it blocked real headlines. Two-source matching is dumber but checkable."
>
> **The receipt.** Replayed against the 14 corrected-headline events in the logs since 2026-05-01: 11 blocked, 3 not, 2 false blocks. The 3 misses were all the syndication case: every configured source was running the same wire copy, so two-source matching agreed on the wrong claim. The 2 false blocks were entries that would have been fine. Both numbers, not just the win.
>
> **What isn't true yet.** The flag is `HEADLINE_GATE`, off by default since merge, flipping to default-on 2026-07-31. It's wired into News Chaser only; no other strategy is touched. Syndication is still open: if every configured source runs the same wire copy, a wrong claim passes anyway. This closes the single-source case, not the syndication case.

*Under the hood: quotes are verbatim from the PR thread. The baseline agent writing this
same post invented a user opt-out that doesn't exist; the skill's evidence-map rule is
what deletes that sentence.*

---

## The knobs (decided by Adrian, 2026-07-30)

1. **Cadence**: shiplog weekly, Monday run. Feature-built on demand when a ship earns
   it. Digest held until the feed surface is picked.
2. **Does the shiplog carry the PnL beat?** No. Ships only. Per-agent performance
   records stay private to their owners, and the real numbers are small and roughly
   flat right now, which makes a weak receipt anyway. When there's a number worth
   showing, the beat comes back (Adrian's call).
3. **Length**: these are on the long end of the range; each format cuts cleanly
   (digest to 3 items, shiplog to anchor + 2). No change requested.
4. **Tier defaults**: confirmed. Tier 2 (Alyna, <24h) is the norm; Tier 3 (Adrian)
   only when the post itself creates new exposure, e.g. the venue rumor above.
5. **Voice**: confirmed. The agent writes plain and doesn't style-match; voice is
   applied by whoever pastes.
6. **Which feed surface and who pastes**: Adrian's call, pending. Note we borrowed
   the formats from Aeon's *Telegram* feed but have only audited their X account
   (`x-account-audit.md` §4); the Telegram audit is the next research task.

**What happens next:** the first live post goes out in the shiplog format, run on real
activity. Posting is by hand on purpose: no Actions runner and no API key until the
format proves out on the real feed (sequencing, decided 2026-07-30). The weekly loop is
a human running the skills against real intake
(`shared-knowledge/marketing/intake/README.md`; v1 is a weekly human paste).
