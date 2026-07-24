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

**Foundation**
- `brand-voice-guard` — enforce tone, banned phrases, confidence calibration, "don't overclaim" + required caveats.
- `claim-evidence-linter` — every assertion must map to a data-drop line, a shipped PR, or a public source. Blocks publish if unmapped claims exist.
- `audience-angle-selector` — one audience per post (trader / builder / founder / partner). Rejects mixed-audience drafts.

**Sense / Data**
- `weekly-signal-intake` — intel + support + product events → ranked opportunities.
- `weekly-data-drop-builder` — receipt-ready factual packet, losses included. *(Phase 1)*

**Make (formats)**
- `format-receipts-post` — core factual post from weekly data: what happened / why / what changed next week. *(Phase 1)*
- `format-build-guide` — "how we did X" from a real implementation path, ending with a reproducible running result.
- `format-fascination-piece` — "we ran N agents with different strategies — here's what happened." Must include uncertainty + failure modes.
- `repurpose-x-thread` — re-argue for X pacing (hook, proof beats, punchy cadence).
- `repurpose-linkedin` — re-argue for LinkedIn narrative (context, insight, CTA).

**Ship / Measure / Learn**
- `publish-tier-router` — score draft risk, route to Tier 1/2/3.
- `utm-tagging-and-ledger` — apply tag schema, log post metadata.
- `monthly-content-retro` — compare format performance, patch skills, open Paperclip updates.

---

## 5. Ship / tiering policy

The marketing analog of the engineers' shape-check gate: *which content is safe enough for the agent to publish alone?*

| Tier | Content | Authority | SLA |
| --- | --- | --- | --- |
| **1 — auto** | Purely factual updates; no forward-looking, partner, legal, or financial claims | Publishes automatically *(post-MVP; draft-only during weeks 1–6)* | — |
| **2 — review** | Receipts interpretations, tactical conclusions | **Alyna** | < 24h turnaround |
| **3 — approval** | Brand strategy shifts, controversial comparisons, partnership/price/legal-sensitive narratives | **Adrian** | As available |

**Hard stops.**
- No evidence map → cannot publish.
- **Kill condition:** if the Tier-2 queue exceeds **14 days** during the first 6 weeks, the experiment ends and we revert to manual. Naming the failure upfront is how we avoid a zombie project.

**Publishing mode, weeks 1–6: draft-only.** All tiers produce finished drafts in Paperclip; a human does the final copy-paste to X/LinkedIn. Live MCP publishing (Tier-1 auto-post, Tier-2 post-on-approval) is a **later phase**, enabled only once the loop is trusted and the draft→paste step is the proven bottleneck.

---

## 6. Rollout (6 weeks) + kill condition

Start with **one skill, one format, validation from day one.** The weekly receipts post off the data drop — the data is ours and the format is factual and low-risk.

| Weeks | Add | Ship mode |
| --- | --- | --- |
| **1–2** | `weekly-data-drop-builder` + `format-receipts-post` | Tier-2 review only, draft-only. Prove one full lap: sense → data → receipts draft → review → tagged link → wiki writeup. |
| **3–4** | `repurpose-x-thread`, `repurpose-linkedin` | Enable Tier-1 *category* (still draft-only). |
| **5–6** | `format-build-guide` | Run first `monthly-content-retro`; patch skills. |

**Rule:** never add a second agent to add a format. Every new format is a new skill file. The moment the instinct is "spin up a video agent / an X agent," that is the platform-per-agent trap. One agent loading skills until we genuinely cross a security boundary or five distinct functions.

**Kill condition (restated):** publishing stalls at the gate for 2 weeks within the first 6 → experiment over, back to manual.

---

## 7. Success metrics

- **Minimum:** clicks + signups per content unit (via tagged links).
- **Ideal:** signup → active-trader conversion.
- **Operational:** Tier-2 SLA adherence; % of drafts published vs. stalled; time-to-publish per tier.
- **Learn-layer:** month-over-month, are promoted formats converting better than pruned ones?

---

## 8. Open questions for Adrian & Nick

1. **Tier boundaries + SLA** — confirm the Tier 1/2/3 split, and the realistic 24h review load on Alyna.
2. **Draft-only → live cutover** — what trigger moves us from human-paste to MCP publishing? (Proposal: after the loop runs clean for the full 6 weeks and draft→paste is the proven bottleneck.)
3. **Canonical Learn-layer KPI** — minimum clicks + signups, or hold out for signup→active-trader conversion? If attribution is thin, the monthly retro has nothing to cut on.
4. **Skill-patch ownership** — who owns the monthly skill patches: Alyna directly, or via Trinity ticketing?

---

## Appendix — the "Make" quality layer (Alyna's mandate)

Section 10 of the how-it-works doc asked specifically for instincts on *what makes content good*. Captured here as design input to the Make skills; to be expanded.

- **Are the three formats right?** Build guide / Receipts / Fascination — which is load-bearing, which to cut or defer, what's missing (e.g. a "we were wrong about X" teardown, a founder-POV format). *[To fill in.]*
- **What makes a receipts post *good* vs. a changelog?** The shareable version vs. the version that reads like release notes. *[To fill in — best captured as a before/after on one real data drop.]*
- **What can an agent make well vs. what always needs a human?** The Make-layer shape-check. *[To fill in.]*
- **What makes you personally stop scrolling?** *[To fill in.]*
