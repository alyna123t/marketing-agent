# Skill-set review — what the reference operators changed

*Date: 2026-07-24. Companion to the PRD. Traces every change in the skill library
back to a specific source, so Adrian & Nick can see the reasoning, not just the result.*

## Sources reviewed
1. **Claire Vo / ChatPRD** — solo founder, nine scoped AI agents. (Solo Founders podcast)
2. **Jacob Bank / Relay.app** — 40 marketing "agents," one human. (EO interview)
3. **Isenberg × Gaskell** — "Building AI Agents that actually work" course.
4. **Andy Lo** — "Fully Autonomous Marketing Team with Claude Code & Skills."
5. **Okara** — commercial "AI CMO," channel-agent product.
6. **Kalash / CrowdReply** — "Claude for Marketing" / AI-search-visibility launch.

## Verdict
Architecture holds (single-agent-many-skills is the consensus pattern). The skill
*set* had three gaps and one efficiency issue. 11 of 13 skills unchanged.

## Changes and why

| Change | Type | Source & reasoning |
| --- | --- | --- |
| Add `weekly-performance-coach` | New skill | Jacob Bank runs a weekly coach ("do more of this, do less of that"); Claire's "Howie" does post-release analytics + recommendations. Two operators run this *weekly*; our only feedback loop was monthly — 4× too slow. |
| Fold `audience-angle-selector` → step + `audiences.md` | Remove skill | "Thin skills, rich knowledge" pattern (Andy Lo, the course). One-audience-per-post is a per-draft checklist item, not a job needing its own runtime. |
| Add `platform-guidelines.md` (knowledge) | New knowledge | Andy Lo keeps a dedicated platform-guidelines file every skill reads; keeps formatting consistent without each skill re-encoding it. |
| Add `audiences.md` (knowledge) | New knowledge | Backs the folded-in audience step. |
| Elevate personality in Tier-2 review | Design | Claire ("rizz is the moat") and Jacob (records own videos for personality) both name founder voice as the un-automatable edge. `brand-voice-guard` enforces guardrails, not charisma — so human review must *add* voice, not just gate risk. Directly answers our "reads like a changelog" failure mode. |
| Measurement marked load-bearing | Emphasis | The coach is only as good as attribution; tagged links are the floor. Confirms our own open question #3. |

## What the sources validated (no change)
- Skills-as-SOPs that compound (course, Andy Lo).
- Draft-for-review as default (Okara drafts nearly everything for approval).
- Start with one, prune ruthlessly (Jacob fires agents; Claire treats formats as a product portfolio with per-format PMF).
- Human owns/defends the output (Claire, via Dan Shipper).
- Marketing is one function → one agent. Claire runs nine agents but exactly **one** marketing agent; her constellation is across *functions*, matching Simmer's existing Simmy/Trinity/Cody split. She validates, not contradicts, the single-agent-for-marketing call.

## Deliberately NOT adopted
- **GEO / AI-search-visibility skill.** Hyped by two clippings (Okara's GEO agent, the CrowdReply launch) but it's a distinct discipline and speculative for a trader/builder audience. **Watch-list, not Phase 1.** Recorded here so the decision is explicit, not an oversight.
- **Channel-per-agent structure** (Okara, Andy Lo's framing). This is the platform-per-agent trap the PRD rejects; their "agents" are really skills. No change.

## One thing to be ready to defend
Claire scopes at the *agent* level (separate identity/tools/coaching per function);
we scope at the *skill* level within one agent. Both reject the mega-agent, so we
agree more than we differ. Our answer to "why not scoped sub-agents?": marketing is
one function, only publishing crosses a security boundary, and multi-agent adds cost
and reasoning fragmentation for no gain below ~5 functions. Already in PRD §2.
