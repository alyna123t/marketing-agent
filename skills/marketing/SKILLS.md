# Marketing skill library: index

Every skill follows [`_SKILL_TEMPLATE.md`](_SKILL_TEMPLATE.md). Build order is
Phase 1 first (see PRD §6). Change rationale in
[`../../docs/specs/2026-07-24-skill-set-review.md`](../../docs/specs/2026-07-24-skill-set-review.md).

## Community feed (current phase: the 2026-07-24 pivot)
The three formats the team chose, borrowed from Aeon's Agent Feed. These are what's
in active use; the X-phase table below is deferred behind them.

| Skill | Group | Status |
| --- | --- | --- |
| `format-weekly-shiplog` | Make | **built + tested** (2026-07-28), ships-only per Adrian's 2026-07-30 call: per-agent PnL stays private, so no public PnL beat; the receipts beat returns when there's a number worth showing |
| `format-tweet-digest` | Make | **built + tested** (2026-07-28): every item carries our line; a bare relay is cut, not shortened. On hold until the feed surface is picked (2026-07-30) |
| `format-feature-built` | Make | **built + tested** (2026-07-28): one ship deep; the trade-off, the rejected alternative, and what still isn't true. Runs on demand |

All three: single fixed audience, no `brand-voice-guard` (voice is team-owned per the
pivot), evidence map required, worth-posting gate can end in "no post."
Test method and fixtures: [`../../test-fixtures/README.md`](../../test-fixtures/README.md).

**Sources (v1):** manual weekly intake, `shared-knowledge/marketing/intake/` (see its
README). Automated ship/tweet sources are deliberately deferred; when one lands, only
each skill's Inputs section changes. Only the Inputs sections change.

## X phase (deferred)

| Skill | Group | Phase | Status |
| --- | --- | --- | --- |
| `weekly-data-drop-builder` | Sense/Data | 1 | contract drafted |
| `format-receipts-post` | Make | 1 | **Paused** (Adrian, 2026-07-24, community-feed-first pivot; was built + tested, kept intact) |
| `brand-voice-guard` | Foundation | 1 | knowledge stub only |
| `claim-evidence-linter` | Foundation | 1 | referenced, not written |
| `weekly-signal-intake` | Sense/Data | 2 | not written |
| `publish-tier-router` | Ship | 1 | not written |
| `utm-tagging-and-ledger` | Measure | 1 | not written |
| `weekly-performance-coach` | Measure/Learn | 2 (wk 3–4) | contract drafted |
| `repurpose-x-thread` | Make | 2 (wk 3–4) | not written |
| `repurpose-linkedin` | Make | 2 (wk 3–4) | not written |
| `format-news-reframe` | Make | 2 (wk 3–4) | not written; repeatable version of our best post ("parlays for prediction markets"): real news + plain-English translation, never a link-drop |
| `engage-replies` | Make/Ship | 2 (wk 3–4) | not written; 3–5 quality replies/day under big accounts; draft-only, Tier 2 (replies carry more tone risk than posts) |
| `format-build-guide` | Make | 3 (wk 5–6) | not written |
| `format-fascination-piece` | Make | later | not written |
| `monthly-content-retro` | Learn | 3 | not written |

## Knowledge files (read by skills, not skills themselves)
| File | Read by | Status |
| --- | --- | --- |
| `shared-knowledge/marketing/brand-voice.md` | `brand-voice-guard` | drafted; **not read by the community-feed skills** (voice is team-owned per the pivot) |
| `shared-knowledge/marketing/platform-guidelines.md` | all `format-*` / `repurpose-*` | drafted; now carries the **Community feed** section (audience, register, no-CTA default) |
| `shared-knowledge/marketing/audiences.md` | X-phase `format-*` | drafted; the four-audience picker does not apply on the community feed, which has one audience by construction |
| `shared-knowledge/marketing/research/x-account-audit.md` | `format-tweet-digest` (§1 watchlist), `format-weekly-shiplog` (§3 the flatline finding) | 10 accounts, two rounds |
| `shared-knowledge/marketing/content-calendar.md` | `weekly-signal-intake`, `publish-tier-router` | drafted; 4-pillar rotation + the **worth-posting gate** (calendar is a ceiling, not a quota; no post ships without something real to say) |

**Folded:** `audience-angle-selector` is no longer a standalone skill; it's a
required "pick one audience" step inside each `format-*` skill, backed by
`audiences.md`. (Thin skills, rich knowledge.)

**Rule:** a new format is a new *skill file*, never a new agent.
