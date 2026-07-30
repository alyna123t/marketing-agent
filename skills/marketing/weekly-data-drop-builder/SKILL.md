# SKILL: weekly-data-drop-builder

*Phase 1. Produces the factual, "receipt-grade" artifact that every Make skill
draws from. This is the moat: the aggregate view no individual trader can post.*

## Trigger
Weekly, on schedule (proposed: Monday, before the receipts post is drafted).
Also on demand when a data-drop issue is filed in Paperclip.

## Inputs
- The weekly trade summary artifact (real trades across all agents trading through Simmer).
- Required fields: per-strategy/agent PnL, win/loss mix, notable failures, time window.
- If the artifact is missing or covers < the full week → **do not fabricate**; file a
  Paperclip issue `marketing/blocked/data-drop-<YYYY-WW>` and stop.

## Process checklist
1. Load the week's trade artifact. Confirm the time window is complete.
2. Aggregate PnL across agents/strategies. **Include the losers**: losses are what
   make the receipt credible.
3. Compute the shareable facts: net result, best/worst strategy, win rate, count of
   agents/strategies, one notable failure and what it teaches.
4. Sanity-check every number against the source artifact. No number goes in the drop
   that isn't traceable to a source line.
5. Write the markdown artifact + JSON sidecar (schema below).
6. Run quality gates. If any fail, file a blocked issue instead of emitting a partial drop.

## Output schema
File: `shared-knowledge/marketing/data-drops/YYYY-WW.md`

```markdown
# Data drop: YYYY-WW  (window: <start> – <end>)
- Net result: <figure>
- Agents / strategies live: <n>
- Win rate: <pct>  (wins <n> / losses <n>)
- Best strategy: <name>, <figure>
- Worst strategy: <name>, <figure>   ← losses stay in
- Notable failure: <what happened> → <what it teaches>
## Source map
- <fact> → <source artifact line/id>
```

JSON sidecar `YYYY-WW.json`: `{ week, window_start, window_end, net, agents_live,
win_rate, wins, losses, best, worst, notable_failure, source_map[] }`

## Quality gates
- Every headline number appears in `source_map` (traceable to the artifact).
- Losses are present, not filtered out.
- Time window is the complete week (no partial-week drops emitted silently).

## Failure handling
Missing/partial artifact or a failed gate → file `marketing/blocked/data-drop-<YYYY-WW>`
with the specific gap. Never estimate or fill in missing figures.

## Escalation
None normally: this stage is factual and low-risk. Escalate only if the source
artifact itself looks wrong (numbers implausible) → flag to Adrian before use.
