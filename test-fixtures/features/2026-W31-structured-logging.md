# PR #421 — Structured logging in SourceMatcher

*Synthetic. Raw PR body + design notes, standing in for the material a "feature built"
deep-dive is written from.*

Author: adrian · Merged: 2026-07-29 · Closes: —

## PR body

Replaces string-formatted log lines in `SourceMatcher` with structured fields
(`claim_id`, `source`, `matched`, `latency_ms`). No behavior change; the gate's
decisions are identical. Makes the logs queryable so we can see match rates per source.

## Design notes (from the PR thread)

**alyna:** LGTM.

## Backtest
None.
