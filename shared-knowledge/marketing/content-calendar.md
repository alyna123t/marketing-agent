# Content calendar — Simmer

*Read by `weekly-signal-intake` (to pick what to draft) and `publish-tier-router`.
The rotation keeps the mix healthy; the worth-posting gate keeps it honest.*

---

## The prime rule: the calendar is a ceiling, not a quota

**No post ships just because the calendar says it's time.** The rotation below caps
how much of each pillar we post — it never obligates a post. If nothing genuinely
interesting exists in a pillar this week, that slot goes empty. Silence beats filler:
every mediocre post trains the audience (and the algorithm) to skip the next good one.

### The worth-posting gate
Every draft must pass ALL of these before it enters the tier router:

1. **Something actually happened.** A real number moved, a real feature shipped, real
   news broke, a real lesson was learned. "It's Tuesday" is not an event.
2. **The send test.** Would a trader or builder plausibly send this to a friend?
   If it only serves *us*, it fails.
3. **One-line value.** State in one line what the reader gets (an edge, an insight,
   a translation of news, a real result). Can't state it → don't post it.
4. **Not a repeat.** Says something the last 10 posts didn't already say.

Failing the gate is a normal, good outcome. The skill logs "no post this slot:
nothing cleared the gate" to the Paperclip issue and stops. That log line is a
feature: it's how we know the gate is working, and the weekly coach tracks how
often each pillar actually clears it.

---

## The four pillars (weekly weights, not slots)

| Pillar | Job | Max share | Fires only when |
| --- | --- | --- | --- |
| **News + reframe** | Reach — ride real news with a plain-English translation ("Parlays, for prediction markets") | ~40% (2–3/wk) | Actual news broke that we can *reframe*, not just relay. No news → nothing. |
| **Receipts** | Proof — the post only we can write; weekly anchor | ~25% (1/wk) | The data drop exists and is complete. Thin/missing data → skip, never pad. |
| **Explainer-lite** | Authority — a little technical, not too technical; why the receipts happened | ~25% (1/wk) | There's a real question readers actually have (from support themes, replies, intel). |
| **Simmer promo** | Conversion — features, updates | ~10% (≤1/wk) | Something actually shipped. Roadmap teases and re-announcements don't count. |

Ratio rationale: ~4:1 value-to-promo. A small brand account that mostly promotes
itself gets ignored — promo earns its reach *because* the other pillars built the
audience.

## Weekly rhythm (default, all slots skippable)

- **Mon:** receipts post (if the data drop cleared its gates) — brand version + founder-voiced variant.
- **Tue–Thu:** news-reframe slots as news actually breaks. Timeliness matters more than the weekday.
- **Any day:** explainer when a real question surfaced; promo when something real shipped.
- **Daily background:** 3–5 quality replies (see `engage-replies`) — replies are the
  reach engine for a small account and are never subject to the calendar.

## Founder-variant rule

Distribution accrues to people, not brand accounts. Every receipts post is drafted
twice from the same evidence map: the brand version and a founder-voiced variant for
Adrian to paste from his own account (his call, per post — see PRD open question #5).

## What the coach watches

`weekly-performance-coach` tracks per-pillar: posts shipped vs. slots skipped, and
performance per pillar. If a pillar keeps failing the gate, that's signal — either
the pillar is wrong or the sensing is (report it, don't force posts through).
