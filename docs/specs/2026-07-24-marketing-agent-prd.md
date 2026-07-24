# PRD — marketing-agent

*Marketing agent for Simmer, built on the existing agent-company framework.*
*Author: Alyna · Date: 2026-07-24 · Status: Draft for Adrian & Nick*

---

## TL;DR

- Simmer's weakest function is marketing. Two prior content pipelines died at the **publish gate** — every post needed Adrian, and it stalled the week he was busy.
- This builds a marketing capability the same way the rest of the team is built: **one agent, many skills**, on Aeon's existing runtime, coordinated through Paperclip, sensing through QMD, gated by a tiered publish check.
- **No new agent per channel.** Every format is a new *skill file*, not a new agent. This is the load-bearing decision and the direct rejection of the "platform per agent" model.
- The moat is the **weekly data drop**: a factual artifact of what Simmer's agents actually traded — real PnL across many strategies, losses included — that no competitor can fake.
- First 6 weeks are **draft-only** (agent writes finished drafts, a human does the final paste). Publishing is **tiered** so nothing waits on one busy person. There is a **named kill condition**.

---

## 1. Problem & goals

**Problem.** Simmer has never staffed marketing; it is the weakest function. Content has been attempted twice and both attempts died at the same place — a single-human publish bottleneck. Meanwhile the platform generates a uniquely credible raw material (aggregate real trading results) that is going unused.

**Goal.** A single scheduled agent that turns Simmer's real trading data and intel into publishable, on-brand content, gated so that no post waits on one busy human.

**Success = a full Sense → Learn lap running every week**, producing attributable signups, without a human bottleneck, and improving month over month as winning formats are promoted into reusable patterns.

**Non-goals.**
- Not a fleet of channel-specialist agents. One agent, skills as stages.
- Not autonomous live posting in the first 6 weeks (draft-only; see §5).
- Not inventing marketing theory — where others (e.g. ChatPRD) have solved this, adopt it.

---

## 2. Architecture

**One agent, many skills.**

| Decision | Choice |
| --- | --- |
| Agent | `marketing-agent` (extends Aeon; not a new runtime) |
| Runtime | Claude Code on GitHub Actions, scheduled — same as Aeon today |
| Coordination | **Paperclip only.** A content idea is an *issue*, not a chat. No agent-to-agent messaging. |
| Knowledge | `shared-knowledge/marketing/` alongside `patterns/` and `intel/` |
| Publishing access | MCP tools, tier-gated — kept off the fully autonomous path, same as Railway and the trading venues |
| Brand consistency | Enforced by a `brand-voice-guard` skill, not by headcount |

**Why single-agent (not multi-agent).** Multi-agent systems run ~15× the token cost of a single agent, compound errors, and fragment step-by-step reasoning through handoffs. A single agent handles ~80% of use cases and is far cheaper and easier to debug. Multi-agent only pays off past ~five distinct functions or across real security boundaries. Marketing is **one** function. We stay single-agent and make quality tunable via skills and versioning, not staffing.

**Skills as stages, not agents as stages.** A content workflow is an assembly line with checkpoints. The platform builders assign each stage to a separate agent; we already have a better primitive — skills. A "LinkedIn post" is a skill, "receipts post" is a skill, "repurpose for X" is a skill. One agent loads whichever skill the current stage needs. This is exactly the existing *"add skills, not agents"* principle.

---

## 3. The six-stage pipeline

The engineering loop (signal → board → triage → shape-check → work → review → learn), pointed at content. Each stage names the skills that fire, its inputs, and its outputs.

### Stage A — Sense · *mostly exists*
Detect what is worth saying this week.
- **Inputs:** `shared-knowledge/intel/*`, Aeon's daily scans, shipped PRs, support themes, competitor movement.
- **Skill:** `weekly-signal-intake`.
- **Output:** Paperclip issue `marketing/opportunity/<slug>` — priority + proposed angle + target audience.

### Stage B — Data drop · *being built now — this is the moat*
Produce factual, "receipt-grade" source material.
- **Inputs:** weekly trade summary artifact; PnL by strategy/agent; win/loss mix; notable failures.
- **Skill:** `weekly-data-drop-builder`.
- **Output:** `shared-knowledge/marketing/data-drops/YYYY-WW.md` + structured JSON sidecar for downstream skills.
- **Why it's the moat:** individual traders post their own results; what Simmer can see that they can't is the *aggregate* — many agents running different strategies at once, with the losses included. A harder-to-fake receipt. The Make skills exist to turn this artifact into shareable formats **first**, before generic content.

### Stage C — Make · *the layer that needs the most work*
Draft platform-native assets from one source of truth.
- **Inputs:** opportunity issue + data drop + brand guardrails.
- **Skills:** `format-receipts-post`, `format-build-guide`, `format-fascination-piece`, `repurpose-x-thread`, `repurpose-linkedin`, gated by `brand-voice-guard`, `claim-evidence-linter`, `audience-angle-selector`.
- **Output:** drafts (X thread, LinkedIn post, build guide, receipts post), **each with a claim→evidence map**.
- **Two load-bearing rules:**
  - **Strategy before volume.** Ten posts targeting the wrong reader are worthless. Make skills deliberately *pause* before inventing positioning or turning one idea into ten shallow posts. The pause is a required step, not an afterthought.
  - **Repurposing = re-argue, not copy-paste.** One receipts post becomes a *threaded* version for X and a *narrative* version for LinkedIn, each written for that reader — adapting argument and pacing, not reposting text.

### Stage D — Ship · *the part that killed the last two attempts*
Publish without bottlenecking on one human. See §5 for full policy.
- **Skill:** `publish-tier-router` scores draft risk → Tier 1 / 2 / 3.
- **First 6 weeks: draft-only.** Agent produces finished, tier-tagged drafts in Paperclip; a human does the final paste. This sidesteps almost all Ship risk while we learn what "good" looks like.

### Stage E — Measure · *exists*
Track outcome per content unit.
- **Inputs:** published URLs + UTM/tag schema.
- **Skill:** `utm-tagging-and-ledger`.
- **Output:** weekly attribution: impressions, clicks, signups, downstream activity.

### Stage F — Learn · *design*
Update skills from real performance.
- **Skill:** `monthly-content-retro`, run monthly.
- **Actions:** prune low performers; promote winners into reusable wiki patterns (nightly extraction promotes a post that did well into a pattern); patch style/claim rules in skills; open Paperclip tickets for the patches.

---

## 4. Skill library

Location: `skills/marketing/`. Every skill follows the same **contract** (see `skills/marketing/_SKILL_TEMPLATE.md`): Trigger · Inputs · Process checklist · Output schema · Quality gates · Failure handling · Escalation. The contract is what stops "agent creativity" from breaking ops consistency.

**Design principle: thin skills, rich knowledge, human review.** Across every reference operator (Andy Lo's brand brain, the Isenberg/Gaskell course, Okara), the leverage is *not* more skills — it's a few thin skills reading rich shared-knowledge files, with a human review step. Prefer a knowledge file + a checklist step inside an existing skill over a new standalone skill. See [`docs/specs/2026-07-24-skill-set-review.md`](2026-07-24-skill-set-review.md) for the source-by-source rationale behind this list.

**Foundation**
- `brand-voice-guard` — enforce tone, banned phrases, confidence calibration, "don't overclaim" + required caveats. Reads `shared-knowledge/marketing/brand-voice.md`.
- `claim-evidence-linter` — every assertion must map to a data-drop line, a shipped PR, or a public source. Blocks publish if unmapped claims exist.
- *(Folded)* **Audience angle** is no longer a standalone skill. It is a required "pick exactly one audience" step inside every `format-*` skill, backed by `shared-knowledge/marketing/audiences.md`. Rationale: one audience per post is a per-draft checklist item, not a job that needs its own runtime.

**Knowledge (not skills — read by the skills above and below)**
- `shared-knowledge/marketing/brand-voice.md` — voice, banned phrases, required caveats, personality cues.
- `shared-knowledge/marketing/platform-guidelines.md` — per-platform specs, structure, and tone (X, LinkedIn, …). Every `format-*` and `repurpose-*` skill reads this so formatting stays consistent without each skill re-encoding it.
- `shared-knowledge/marketing/audiences.md` — the four audiences (trader / builder / founder / partner) and how to write for each.

**Sense / Data**
- `weekly-signal-intake` — intel + support + product events → ranked opportunities.
- `weekly-data-drop-builder` — receipt-ready factual packet, losses included. *(Phase 1)*

**Make (formats)**
- `format-receipts-post` — core factual post from weekly data: what happened / why / what changed next week. *(Phase 1)*
- `format-build-guide` — "how we did X" from a real implementation path, ending with a reproducible running result.
- `format-fascination-piece` — "we ran N agents with different strategies — here's what happened." Must include uncertainty + failure modes.
- `repurpose-x-thread` — re-argue for X pacing (hook, proof beats, punchy cadence).
- `repurpose-linkedin` — re-argue for LinkedIn narrative (context, insight, CTA).
- `format-news-reframe` — real news + a plain-English translation ("Parlays, for prediction markets" — our best-performing post, made repeatable). Never a link-drop; the reframe is the post. Highest-reach pillar for a small account.
- `engage-replies` — 3–5 sharp, receipt-backed replies/day under large accounts (Polymarket, Kalshi, CT traders). For a small account, replies borrow big audiences and outperform posting into the void. Draft-only, Tier 2 — replies carry more tone risk than posts.

**Content mix & the worth-posting gate.** Posting follows a four-pillar weighted rotation (news-reframe ~40% / receipts ~25% / explainer ~25% / promo ~10%) defined in `shared-knowledge/marketing/content-calendar.md` — roughly 4:1 value-to-promo. The calendar is a **ceiling, not a quota**: no post ships just because it's Tuesday. Every draft must pass the worth-posting gate (something actually happened; a reader would send it to a friend; states its value in one line; isn't a repeat). An empty slot is a normal outcome and gets logged; the weekly coach tracks how often each pillar clears the gate. Silence beats filler — every mediocre post taxes the reach of the next good one.

**Founder-variant rule.** Distribution accrues to people, not brand accounts (every reference operator agrees). The receipts skill drafts each post twice from the same evidence map: brand version + a founder-voiced variant for Adrian's account. Whether he uses it is his call per post (open question #5).

**Ship / Measure / Learn**
- `publish-tier-router` — score draft risk, route to Tier 1/2/3.
- `utm-tagging-and-ledger` — apply tag schema, log post metadata.
- `weekly-performance-coach` — **new.** Reads the attribution ledger and tells the operator what to double down on and what to drop this week ("do more of this, do less of that"). Recommender, not a pruner. Added because two reference operators (Jacob Bank's weekly AI coach, Claire Vo's analytics agent "Howie") run exactly this loop *weekly* — the monthly retro alone is 4× too slow to steer. Depends on Measure being richer than raw tagged links (see §7 and open question #3).
- `monthly-content-retro` — the deep pass: compare format performance, prune losers, promote winners into `patterns/`, patch skills, open Paperclip updates.

---

## 5. Ship / tiering policy

The marketing analog of the engineers' shape-check gate: *which content is safe enough for the agent to publish alone?*

| Tier | Content | Authority | SLA |
| --- | --- | --- | --- |
| **1 — auto** | Purely factual updates; no forward-looking, partner, legal, or financial claims | Publishes automatically *(post-MVP; draft-only during weeks 1–6)* | — |
| **2 — review** | Receipts interpretations, tactical conclusions | **Alyna** | < 24h turnaround |
| **3 — approval** | Brand strategy shifts, controversial comparisons, partnership/price/legal-sensitive narratives | **Adrian** | As available |

**Review adds voice, it doesn't only gate risk.** Every reference operator says the durable human edge is personality — Claire Vo's "rizz is the moat," Jacob Bank recording his own videos because they express *his* personality. `brand-voice-guard` enforces guardrails (tone, banned phrases); it cannot supply charisma. So the Tier-2 human review is explicitly the step where a person *adds* the point of view, the aside, the stopping-power — not just checks for danger. A draft that clears every gate but reads like a changelog has failed. (See the Make appendix.)

**Hard stops.**
- No evidence map → cannot publish.
- **Kill condition:** if the Tier-2 queue exceeds **14 days** during the first 6 weeks, the experiment ends and we revert to manual. Naming the failure upfront is how we avoid a zombie project.

**Publishing mode, weeks 1–6: draft-only.** All tiers produce finished drafts in Paperclip; a human does the final copy-paste to X/LinkedIn. Live MCP publishing (Tier-1 auto-post, Tier-2 post-on-approval) is a **later phase**, enabled only once the loop is trusted and the draft→paste step is the proven bottleneck.

---

## 6. Rollout (6 weeks) + kill condition

Start with **one skill, one format, validation from day one.** The weekly receipts post off the data drop — the data is ours and the format is factual and low-risk.

| Weeks | Add | Ship mode |
| --- | --- | --- |
| **1–2** | `weekly-data-drop-builder` + `format-receipts-post` (+ `platform-guidelines.md`, `audiences.md`) | Tier-2 review only, draft-only. Prove one full lap: sense → data → receipts draft → review → tagged link → wiki writeup. |
| **3–4** | `repurpose-x-thread`, `repurpose-linkedin`, `weekly-performance-coach` | Enable Tier-1 *category* (still draft-only). Coach starts steering once ≥2 weeks of attribution exist. |
| **5–6** | `format-build-guide` | Run first `monthly-content-retro`; patch skills. |

**Rule:** never add a second agent to add a format. Every new format is a new skill file. The moment the instinct is "spin up a video agent / an X agent," that is the platform-per-agent trap. One agent loading skills until we genuinely cross a security boundary or five distinct functions.

**Kill condition (restated):** publishing stalls at the gate for 2 weeks within the first 6 → experiment over, back to manual.

---

## 7. Success metrics

- **Minimum:** clicks + signups per content unit (via tagged links).
- **Ideal:** signup → active-trader conversion.
- **Operational:** Tier-2 SLA adherence; % of drafts published vs. stalled; time-to-publish per tier.
- **Learn-layer:** month-over-month, are promoted formats converting better than pruned ones?

**Measurement is now load-bearing, not just a scoreboard.** `weekly-performance-coach` can only recommend if per-unit attribution is recorded weekly. Tagged links are the floor; if attribution stays thin, the coach degrades to noise and the whole Learn loop has nothing to steer on. Treat richer attribution (per-post → signup, ideally → active) as a Phase-1 requirement, not a nice-to-have. This is the same concern as open question #3.

---

## 8. Open questions for Adrian & Nick

1. **Tier boundaries + SLA** — confirm the Tier 1/2/3 split, and the realistic 24h review load on Alyna.
2. **Draft-only → live cutover** — what trigger moves us from human-paste to MCP publishing? (Proposal: after the loop runs clean for the full 6 weeks and draft→paste is the proven bottleneck.)
3. **Canonical Learn-layer KPI** — minimum clicks + signups, or hold out for signup→active-trader conversion? If attribution is thin, the monthly retro has nothing to cut on.
4. **Skill-patch ownership** — who owns the monthly skill patches: Alyna directly, or via Trinity ticketing?
5. **Founder-account posting** — is Adrian willing to paste a founder-voiced variant of the weekly receipts post from his own account? Personal accounts reliably out-distribute brand accounts (Claire Vo, Jacob Bank both lean on this); the agent drafts both versions either way, so the cost is zero — but it's his name, so it's his call.

---

## Appendix — the "Make" quality layer (Alyna's mandate)

Section 10 of the how-it-works doc asked specifically for instincts on *what makes content good*. Captured here as design input to the Make skills; to be expanded.

The clippings sharpen this: two operators independently name **personality / founder voice as the un-automatable moat** ("rizz is the moat"; recording your own videos to express your personality). That reframes the whole appendix — the agent's job is to get a draft 90% of the way on *facts and structure*; the human review is where the last 10% of *voice* goes in. The questions below are how we define that 10%.

- **Are the three formats right?** Build guide / Receipts / Fascination — which is load-bearing, which to cut or defer, what's missing (e.g. a "we were wrong about X" teardown, a founder-POV format — the latter now explicitly supported by the personality finding). *[To fill in.]*
- **What makes a receipts post *good* vs. a changelog?** The shareable version vs. the version that reads like release notes. *[To fill in — best captured as a before/after on one real data drop.]*
- **What can an agent make well vs. what always needs a human?** The Make-layer shape-check. Working answer from the clippings: agent owns facts + structure + platform formatting; human owns voice + point of view. *[To refine.]*
- **What makes you personally stop scrolling?** First answer now lives in [`shared-knowledge/marketing/brand-voice.md`](../../shared-knowledge/marketing/brand-voice.md): outcome-led plain English over feature names, real numbers over hype, honesty as the hook ("you can still lose"), and the news-reframe pattern ("parlays, for prediction markets"). Built from our past posts vs. Elastics + hook research; refine from real performance.
