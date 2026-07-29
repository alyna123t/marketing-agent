# Intake — the v1 pipeline is a human paste

No automated ship source or tweet-capture pipeline exists yet. Until one does, the
pipeline is this folder: **once a week, a human pastes raw material in; the agent reads
only what's here.** Resist making the agent go fetch — v1 keeps the source boundary
explicit so we can swap in automation later by changing only the skills' Inputs sections.

## What to paste, weekly

| File | What goes in | Shape to copy |
| --- | --- | --- |
| `YYYY-WW-ships.md` | The week's merged PRs (number, title, author, merge date, first lines of body), open/unmerged PRs, and issue movement | `test-fixtures/ship-logs/2026-W30-ships.md` |
| `YYYY-WW-tweets.md` | The week's captures from the watchlist (`research/x-account-audit.md` §1): account, date, abridged text, engagement, permalink | `test-fixtures/tweet-captures/2026-W30-captures.md` |
| `features/<pr-id>-<slug>.md` | Only when a ship earns a deep-dive: the PR body, the design discussion from the thread (verbatim, with names), the linked issue, any backtest | `test-fixtures/features/2026-W30-headline-verification.md` |

Paste **raw material only** — no editorializing, no pre-filtering beyond the watchlist,
no "this one's important" annotations. The judgment lives in the skills; an annotated
intake file quietly becomes the post and bypasses the gates.

## Rules

- **Week key is ISO week** (`2026-W31`), matching `data-drops/`.
- **A missing file is a fact, not an error.** The agent writes a blocked note to
  `drafts/community-feed/` and stops — it never scrapes, guesses, or reuses last week.
- **Thin is fine.** If the week was quiet, paste the quiet week. The worth-posting gate
  deciding "no post" is the system working.
