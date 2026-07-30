# SKILL: weekly-performance-coach

*New. A weekly recommender, not a pruner. Reads the attribution ledger and tells
the operator what to double down on and what to drop. Added because two reference
operators run exactly this loop weekly (Jacob Bank's AI coach; Claire Vo's "Howie"
analytics agent); the monthly retro alone is 4× too slow to steer content.*

## Trigger
Weekly, on schedule (proposed: Friday, after the week's posts have ≥48h of data).
Starts producing signal only once ≥2 weeks of attribution exist.

## Inputs
- The attribution ledger from `utm-tagging-and-ledger` (per-post: impressions, clicks, signups, downstream activity).
- The week's published/queued content units and their format + audience tags.
- Prior weeks' coach outputs (to track whether last week's advice was taken and worked).
- If < 2 weeks of attribution exist → emit "insufficient data, holding recommendations" and stop. Do not invent trends from one week.

## Process checklist
1. Load the ledger for the trailing 2–4 weeks.
2. Rank content units by the canonical KPI (clicks + signups floor; signup→active if available).
3. Identify the top pattern to **double down on** (format × audience × angle) and the one to **drop or pause** this week.
4. Sanity-check against sample size: do not over-fit to a single viral or dud post; flag low-confidence calls as such.
5. Write 3–5 concrete, actionable recommendations for next week ("more X, less Y"), each tied to the evidence.
6. File as a Paperclip issue for the operator; do NOT auto-change skills (that is the monthly retro's job).

## Output schema
A Paperclip issue `marketing/coach/YYYY-WW`:
```
# Performance coach: YYYY-WW  (window: trailing <n> weeks)
Double down on: <format × audience × angle>, <evidence: metric, sample size>
Drop / pause:   <format × audience × angle>, <evidence>
Recommendations for next week:
  1. <do more of this>, because <metric>
  2. <do less of this>, because <metric>
Confidence: <high / low + why>
Last week's advice: <taken? did it work?>
```

## Quality gates
- Every recommendation cites a ledger metric with its sample size.
- No trend claimed from a single data point.
- Low-confidence calls are labelled, not laundered as certainty.

## Failure handling
Missing/thin ledger → emit "insufficient data" and stop; never fabricate trends.
Attribution obviously broken (all-zero, implausible) → file a blocked issue flagging the instrument, not the content.

## Escalation
Advisory only: files an issue, changes nothing. Escalate to `monthly-content-retro`
(and the operator) if the same "drop this" signal recurs 3+ weeks: that is a format
to actually prune, which is a skill change and belongs to the monthly pass.
