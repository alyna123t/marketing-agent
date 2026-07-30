# PR #412 — Headline-verification gate for News Chaser

*Synthetic. Raw PR body + design notes + linked issue, standing in for the material a
"feature built" deep-dive is written from.*

Author: adrian · Merged: 2026-07-24 · Closes: FIXT-231

## PR body

Requires a second independent source for a headline claim before a News Chaser agent
may enter. Implementation:

1. Headline arrives from the news feed; `ClaimExtractor` reduces it to a single claim sentence.
2. `SourceMatcher` queries the other configured feeds for the same claim, window `HEADLINE_GATE_WINDOW_S = 90`.
3. Match → entry proceeds. No match → entry blocked, reason written to `agent.log`.
4. Conflicting sources → stand down, headline added to the agent's do-not-retry set.

Feature-flagged `HEADLINE_GATE`, default off. Flip to default on 2026-07-31.
Wired into News Chaser only; other strategies unaffected.

## Design notes (from the PR thread)

**alyna:** why 90s?
**adrian:** longer window catches more corrections but the edge on a real headline is
mostly gone by ~2min. 90 is where we still catch most corrections without giving up all
the edge. Tunable, not derived — I expect we move it.

**alyna:** did you consider just asking a model whether the headline looks true?
**adrian:** yes, rejected. Non-deterministic, can't audit why it fired, and in the spike
it blocked real headlines. Two-source matching is dumber but checkable.

**alyna:** what about syndication?
**adrian:** known hole. If every source runs the same wire copy, two-source matching
passes a wrong headline. This fixes the single-source case, not the syndication case.

**alyna:** why default off?
**adrian:** don't want to change every live agent's behavior mid-week without warning.

## Linked issue FIXT-231
"Agents enter on unverified headlines." Opened 2026-07-19 after the W29 review.

## Backtest attached to the PR
Replay against the 14 corrected-headline events in our logs since 2026-05-01:
- Entry blocked: 11 of 14.
- Not blocked: 3 (all sources carried the same wrong claim — syndication case).
- False blocks: 2 entries that would have been fine.
Source: `ledger/backtests/2026-07-24-headline-gate.csv`
