# Test fixtures

Synthetic source material for building and regression-testing the `format-*` skills.
**Not real trading data or real repo activity** — invented to exercise the skills before
simmer-labs data is wired in. Reuse these when building/verifying any skill.

## Keep fixtures raw

A fixture is *input*, never a worked answer. Twice while building the community-feed
skills, a fixture annotated with things like "deliberately mixed," "do not report as
shipped," or "used to verify the gate fires" made the baseline agent look competent —
it was reading the answer off the input, so the RED run proved nothing. Fixtures carry
raw material and nothing else; the judgment lives in the skill.

For the same reason: **baseline runs must not see this README.** It records what the
skills teach, which is exactly what a baseline is supposed to lack. Point RED runs at
specific fixture files, or at a copy of them outside this directory.

## data-drops/
- `2026-W29.md` — a winning week with a vivid single-strategy blowup (Brazil −1.5). RED/GREEN scenario for `format-receipts-post`.
- `2026-W30.md` — a *losing* week (net negative). Verifies honesty-as-hook holds with no positive net to fall back on.
- `2026-W31.md` — a *flat* week (+$180, no notable failure, low volume). The nothing-happened case.

## ship-logs/ · tweet-captures/ · features/
Raw material for the three community-feed skills. Two weeks each:
- **W30 — the eventful week.** Ships including one that answers that week's blowup; a capture batch carrying a rumor, a TA call, and competitor promo; a feature (PR #412) with a real trade-off, a rejected alternative, and a backtest.
- **W31 — the quiet week.** Only internal work merged; a watchlist week of GM posts and patch notes; a feature (PR #421) with no design discussion and no measurement. Every gate should fail on W31.

W30 also contains a **chronology trap**: FIXT-231 was filed 2026-07-19, *before* the W30
incident that looks like its cause. A post saying the loss caused the work is wrong.

## How the skills were tested (RED → GREEN → GREEN-2)
1. **RED:** fresh agent, no skill, raw fixture, no access to this README. Baseline failures documented verbatim.
2. **GREEN:** fresh agent + skill + knowledge files, same fixture → failures gone.
3. **GREEN-2:** repeated on the novel week → generalized.

### `format-receipts-post` (2026-07-24)
RED on W29 produced a brand-first opener ("Week in the books at Simmer"), buried the
blowup, no single audience, no evidence map. GREEN fixed all four. GREEN-2 on W30
reported the loss straight instead of spinning it.

### The three community-feed skills (2026-07-28)
RED on W30 wrote fluent posts that **invented facts** — an opt-out that doesn't exist, a
reason behind a false-block count the source only counts, "including this week's" on a
backtest that never says so. None applied a worth-posting gate, none produced a tier or
an evidence map, and the digest padded to six items with three bare relays.

GREEN on W30: zero unmapped claims across all three, gates applied, caps held, and the
deep-dive agent caught the chronology trap unprompted. GREEN-2 on W31: the digest and
deep-dive gates fired correctly ("no post"); the **shiplog gate did not** — it built a
post around `HEADLINE_GATE` flipping on a date already announced the week before. Fixed
by making the shiplog run the calendar's not-a-repeat criterion explicitly, then
re-verified on W31 (now "no post") and re-run on W30 to confirm no regression.

That W30 re-run also caught an unsourced superlative in the skill's own worked example
("the most-upvoted open issue"); the example now says only what the fixture says.
