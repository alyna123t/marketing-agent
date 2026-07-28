# Platform guidelines — Simmer

*Read by every `format-*` and `repurpose-*` skill so platform formatting stays
consistent without each skill re-encoding it. Pattern borrowed from Andy Lo's
"platform guidelines" brand-brain file. Fill specifics from what actually performs;
the monthly retro updates this.*

## X / Twitter
- **Shape:** thread. Hook in post 1 (must stand alone), one proof beat per post, punchy cadence.
- **Hook rule:** lead with the hardest-to-fake number or the most counterintuitive result — including a loss.
- **Length:** short posts; no walls of text. Numbers over adjectives.
- **Caveats:** window + sample size stated once (thread-level), not per post.
- **CTA:** one, at the end. Tagged link only.

## LinkedIn
- **Shape:** single narrative post. Context → insight → what it means for the reader.
- **Voice:** slower, more explanatory than X. Still specific, still one audience.
- **Length:** medium; front-load the insight so it survives the "see more" fold.
- **Caveats:** stated inline where the claim is made.
- **CTA:** one, soft. Tagged link.

## Community feed (internal — the current phase)
*Our own community feed, not a public timeline. Read by `format-weekly-shiplog`,
`format-tweet-digest`, `format-feature-built`. Added 2026-07-28 for the
community-feed-first pivot.*

- **Shape:** one post, short paragraphs, bold labels instead of headers. No emoji-per-line, no slide-deck formatting.
- **Audience:** always the same one — **existing users running agents with real money.** The four-audience picker in `audiences.md` does not apply here; the feed has one audience by construction. Write for someone whose own money is affected by what you're reporting.
- **Register:** engineering-honest. Numbers, real names (PRs, flags, strategies, issue IDs), no pitch. This audience already bought; selling to them costs trust.
- **Brand voice:** **not the agent's job.** Per the 2026-07-24 pivot, brand-voice matching is team-owned. Do **not** run `brand-voice-guard` and do not restyle for voice — write plainly and let human review handle voice.
- **Length:** the shortest version that carries the evidence. Cut anything a reader can't act on or check.
- **CTA:** none by default. This is a feed for people already using the product. An action line belongs only when the reader has an action available *and the source proves it exists* — "a flag flips on Thursday" is information the reader can't act on and is not a CTA. When a `format-*` skill is stricter about this, the skill wins.
- **Losses and limits stay in** — same invariant as every other platform, and it matters more here because these readers can verify it against their own accounts.

## Repurposing rule (applies to all platforms)
Re-argue, do not copy-paste. One receipts post becomes a *threaded* X version and a
*narrative* LinkedIn version — adapt argument and pacing per reader, never repost the
same text. *(This is the standing rule from PRD §3 Stage C.)*

## Shared invariants
- Every claim maps to evidence (`claim-evidence-linter` runs regardless of platform).
- Exactly one target audience per post (see `audiences.md`; on the community feed the audience is fixed).
- Losses stay in. Credibility over polish.

## Platforms not yet in scope
Threads, YouTube, Reddit, GEO/AI-search — *[watch-list; add a section here only when a platform enters scope, never a new agent.]*
