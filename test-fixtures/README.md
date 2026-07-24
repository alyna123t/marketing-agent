# Test fixtures

Synthetic data drops for building and regression-testing the `format-*` skills.
**Not real trading data** — invented to exercise the skills before simmer-labs data
is wired in. Reuse these when building/verifying any skill that consumes a data drop.

## data-drops/
- `2026-W29.md` — a winning week with a vivid single-strategy blowup (Brazil −1.5). Used as the RED/GREEN scenario when building `format-receipts-post`.
- `2026-W30.md` — a *losing* week (net negative). Used to verify the skill generalizes: honesty-as-hook must hold when there's no positive net to fall back on.

## How the receipts skill was tested (RED → GREEN)
1. **RED:** fresh agent, no skill, same W29 data → produced a brand-first opener ("Week in the books at Simmer"), buried the blowup, no single audience, no evidence map.
2. **GREEN:** fresh agent + skill + knowledge files → led with the blowup, single audience, full evidence map, founder variant, gate reasoning.
3. **GREEN-2:** repeated on W30 (losing week, novel) → generalized; reported the loss straight instead of spinning it.
