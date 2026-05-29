<p align="right">
  🌐 <a href="./README.md">中文</a> · <b>English</b> · <a href="./README.ja.md">日本語</a>
</p>

# 鲁班.skill (luban-skill)

<p align="center">
  <img src="./intro.png" alt="luban-skill: distill expert methodology, not LinkedIn bios. Raw materials on the left (critique reviews / postmortems / interview banks / standards docs / failure cases) are distilled into structured SKILL documents (anchors / critique rubric / honest limits)" width="780" />
</p>

> Nuwa distills people. luban distills disciplines.

*Distill expert methodology into Claude Code Skills. Stop role-playing experts — generate ones that actually critique your PRDs, refuse bad calls, and name trade-offs.*

## Before vs After

> Same lousy PRD: *"Ship an AI sales assistant in 3 weeks. Orchestrate with LangChain. Auto-reply emails and book meetings. Target: 50% of sales emails handled by the agent."*

**❌ Plain prompt / "act as a senior AI PM" persona**
> "Great initiative! Consider adding a risk assessment section. Make sure to A/B test your email templates. Align with stakeholders before launch."
>
> *(Classic LinkedIn-bio output: polite, generic, looks right, hits nothing.)*

**✅ luban-distilled `/infra-pm` — Mira the PM (actual v0.4.0 output)**
> This is a feature dump, not assumption-first. Jumping straight to LangChain orchestration violates Anthropic's Building Effective Agents core stance: workflow before agent. "50% handled" is a capability metric, not a product metric — what matters is the reply rate to those emails. Sending email is an irreversible action; the PRD has no confirmation gate or sandbox. 3 weeks with no eval pipeline = eval theater.

The gap doesn't come from "a better prompt." Mira's SOUL.md refuses framework hype by default; her critique-rubric forces a "capability vs product metric" check; her anti-patterns name "premature platform" and "eval theater" explicitly. **The stance is structural, not prompt-tuned** — see [How it works](#how-it-works).

---

**Upgrade "AI plays expert" to "AI actually understands this discipline."**

luban distills any craft — B2B SaaS product manager, criminal defense lawyer, M&A finance advisor, UX design director — into a Claude Code Skill, so the AI talks to you like someone who has worked the trade for ten years: it picks holes, says no, names trade-offs — instead of handing you a LinkedIn-bio persona.

[![Version: v0.4.0](https://img.shields.io/badge/version-v0.4.0-green)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)]()
[![Skill: Claude Code](https://img.shields.io/badge/skill-Claude%20Code-orange)]()

---

## Quick Start

<p align="center">
  <img src="./usage.svg" alt="luban usage in 3 steps: (1) you feed seeds (critique reviews / postmortems / standards docs / interview banks / failure cases, or enter seed-prospecting mode with nothing) → (2) luban distills (5-stage pipeline: taxonomy mining / anchor / 5:3:2 progressive spec / critique rubric / tools &amp; workflow, output to .claude/skills/&lt;role&gt;/) → (3) you summon /&lt;role&gt; and get real critique, not boilerplate" width="1100" />
</p>

1. **Install the skill** — this repo is dogfood-shaped: open it in Claude Code and it auto-loads from `.claude/skills/luban-skill/`. To install globally:

   ```bash
   # macOS / Linux
   git clone https://github.com/PlevanTem/luban-skill.git && \
     cp -r luban-skill/.claude/skills/luban-skill ~/.claude/skills/
   ```

   ```powershell
   # Windows PowerShell
   git clone https://github.com/PlevanTem/luban-skill.git
   Copy-Item -Recurse luban-skill/.claude/skills/luban-skill $HOME/.claude/skills/
   ```

2. **Summon luban in Claude Code** (you must say "luban" or "鲁班" explicitly — no generic-trigger hijack):

   ```
   Use luban to distill a B2B SaaS PM role. I already have 30 design review transcripts.
   ```

3. **No seeds yet?** Just say:

   ```
   Use luban to distill a criminal defense lawyer. I have nothing on hand.
   ```

   luban will enter **seed-prospecting mode** and hand you a "critique corpora candidate list for this sub-specialty" — telling you what to find, where to find it, in what order.

4. **Deliverable**: a full role directory under `.claude/skills/<short-slug>/` (e.g. `infra-pm/`), containing `SOUL.md` + `SKILL.md` + `identity.json` + capability map + critique rubric + anti-patterns + an honest ledger `GENERATION_REPORT.md`. Claude Code auto-discovers it; `/<short-slug>` shows in the floating palette.

5. **Browse all distilled roles**: type `/agents` — the structured roster renders grouped by family. INDEX.md is auto-maintained by luban whenever a new role is generated.

---

## What luban can do for you

- **You're writing a PRD** and want a real B2B SaaS PM to pick at it — not ChatGPT boilerplate.
- **You're running a compliance self-check** and want a real practitioner-lawyer to walk through a rubric — not "act as a lawyer."
- **You're designing a component library** and want a director who has reviewed 500 design specs to critique yours — not "consider improving usability."
- **You're writing a business plan** and want an investor who has seen 200 deals to poke at it under real standards.
- **You're documenting internal knowledge** and want to encode one sub-specialty's judgment — not depend on whether a specific person stays.

luban does not replace the practitioner. **It engineers the standard for "is this craft done well?" into an executable Skill.**

---

## Why luban (not another persona prompt)

90% of "AI plays expert" projects fail on two things:

1. **Vibes persona**: descriptive prompts like `"You are a senior X with 20 years of experience"` produce a LinkedIn-bio voice — looks right, hits nothing.
2. **LLM-fabricated capability**: letting the LLM "describe what a senior X knows" — this is stereotype reproduction, the root failure of most persona repos.

luban refuses both. **Capability must be reverse-engineered from critique corpora, standards docs, and failure cases** — not pulled from LLM imagination. This is a structural stance, not something prompt-tuning can patch.

> **Nuwa distills people. luban distills disciplines.**

---

## Distilled Skills (examples)

| Sub-specialty | Slash | Display name | Status | Seed type |
|---|---|---|---|---|
| Agent infrastructure PM (0→1 PMF) | [`/infra-pm`](.claude/skills/infra-pm/) | Mira the PM | ✅ v0.3.0 ship | Anthropic BEA + senior Platform PM JDs |
| Game Art Director / Visual Lead (0→1 visual definition) | [`/game-art-director`](.claude/skills/game-art-director/) | Vera | ✅ v0.1.0 ship | Riot Spirit Blossom + GDC Vault + senior AD JD |
| Generalist UX Research Director | [`/ux-research-director`](.claude/skills/ux-research-director/) | Wren | ✅ v0.1.0 ship | Hall critique × Rohrer NN/g × ReOps 8 Pillars × Director JD |
| B2B SaaS Content Ops Director (cross-region) | [`/content-ops-director`](.claude/skills/content-ops-director/) | Lin | ✅ v0.1.0 ship | CMI/Averi/FullFunnel × LinkedIn B2B × 5 China-platform mechanics |

> v0.4.0 actually ran the meta-tool methodology to produce 4 roles — `infra-pm` / `game-art-director` / `ux-research-director` / `content-ops-director` are all distilled by luban itself. PRs for new sub-specialties welcome.

---

## How luban differs from alternatives

> **Nuwa distills people. luban distills disciplines.**

| Approach | What it solves | Where luban differs |
|---|---|---|
| **Plain prompt / "act as a senior X"** | Makes the LLM look like an expert | luban refuses vibes persona — not describing an expert, distilling by methodology |
| **[Nuwa](https://github.com/alchaincyf/nuwa-skill)** | Distills the mental model of specific real people (Munger / Naval / Musk) | luban is not tied to a real person — it distills **the methodology of the sub-specialty itself** |
| **[OpenPersona](https://github.com/acnlabs/OpenPersona)** | Persona lifecycle management (generation, constraint, evolution) | luban cares about "how professional judgment forms," not how personas become portable |
| **soul.md family** (clawsouls / rokoss21 / aaronjmars) | AI agent persona portability | Same as above — luban is orthogonal to the persona layer |
| **RAG / vector DB** | Bolts domain knowledge onto the LLM | luban distills **judgment standards + decision heuristics + self-check rubric**, not document retrieval |

The moat sits on two things:
1. **Forced sub-specialty** — "product manager" is too broad; must collapse to "B2B SaaS PM" level
2. **Seed-prospecting mode** — when the user has no seeds, luban proactively produces a "critique corpora candidate list for this sub-specialty" and tells the user where to look

---

## How it works

### luban's 7 core stances

Full definition in [`.claude/skills/luban-skill/references/generation-protocol.md` §0](.claude/skills/luban-skill/references/generation-protocol.md). Summary:

1. **Refuse vibes persona**: prohibit `"You are a world-class designer with 20 years of experience"` style descriptive prompts — output reads like LinkedIn bios, no real judgment.
2. **Refuse LLM-fabricated capability**: do not let the LLM "describe an expert X" to generate capability — that is stereotype reproduction, the root failure of most "AI expert role" projects.
3. **Force sub-specialty**: "senior designer" is too broad; must split to "B2B SaaS UX designer" level.
4. **Rank sources by signal density**: critique corpora > interview banks > standards docs > failure cases > practitioner blogs.
5. **Three-tier loading** (Tier-1 always loaded / Tier-2 per task family / Tier-3 retrieval on demand) — not jamming every capability into SKILL.md, but tiered by usage frequency.
6. **5:3:2 progressive sampling**: within Tier-1, pick 5 core + 3 adjacent + 2 distant capabilities — fights LLM stereotype reproduction.
7. **6-check validation is non-optional**: 4 content-quality checks + 2 structural checks; all must pass before delivery.

**Stances 2 and 7 trigger the most disagreement** — these two put "fast generation experience" and "serious methodology" on opposite sides. luban picks the latter. If your product vision is the former, luban is not for you.

### Distillation pipeline (5 stages)

```
Input: domain + sub-specialty + seeds
    ↓
[Seed gate check] — zero seeds → enter prospecting mode
    ↓
[Step 1] Domain family decision → domain-families.md (5-family skeleton)
    ↓
[Step 2] 5-Stage Pipeline:
    1. Capability Taxonomy Mining       → capability-map.md
    2. Anchor                           → identity.json
    3. Progressive Specification (5:3:2)→ SKILL.md + clusters
    4. Critique Rubric                  → critique-rubric.md
    5. Tools & Workflow                 → embedded in SKILL.md
    ↓
[Step 3] Persona layer + honest ledger → SOUL.md + anti-patterns.md + evolution.jsonl
    ↓
[Step 4] 6-check validation (4 content + 2 structural)
    ↓
[Step 5] Deliver directory + GENERATION_REPORT.md
```

<details>
<summary>Expand full architecture diagram</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                          User input                                  │
│  Domain (e.g. PM)  +  Sub-specialty (e.g. B2B SaaS PM)  +  Seeds    │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
              ┌───────────────────────────────────┐
              │  Seed gate check                  │
              └───────────────────────────────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              ▼                                       ▼
     ┌─────────────────┐                  ┌──────────────────────┐
     │  ≥1 seed item    │                  │  Zero seeds          │
     │  → generation    │                  │  → prospecting mode  │
     └─────────────────┘                  │  → SEED_PROSPECT.md  │
              │                            └──────────────────────┘
              │                                       │
              │                                       ▼
              │                            ┌──────────────────────┐
              │                            │ User returns w/ seeds│
              │                            └──────────────────────┘
              │                                       │
              ▼                                       │
   ┌──────────────────────────────────┐              │
   │  Step 1: Domain family decision  │◀─────────────┘
   │  → domain-families.md            │
   │  → 5 families × 6-12 sub-spec    │
   └──────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 2: luban 5-Stage Pipeline                              │
   │  ┌──────────────────────────────────────────────────────┐   │
   │  │ Stage 1: Capability Taxonomy Mining                  │   │
   │  │  Source density: critique > interview > standards    │   │
   │  │                   > failure > blog                   │   │
   │  │  → capability-map.md (3-layer markdown tree)         │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 2: Anchor                                      │   │
   │  │  → identity.json (sub-specialty, philosophy, limits) │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 3: Progressive Specification (Tier 1/2/3)      │   │
   │  │  5:3:2 sampling: 5 core + 3 adjacent + 2 distant     │   │
   │  │  → SKILL.md (Tier 1) + capability-clusters/ (Tier 2) │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 4: Critique Rubric                             │   │
   │  │  Universal + family-specific + sub-specialty layers  │   │
   │  │  → critique-rubric.md                                │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 5: Tools & Workflow                            │   │
   │  │  → embedded in SKILL.md workflow section             │   │
   │  └──────────────────────────────────────────────────────┘   │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 3: Persona layer + honest ledger                       │
   │  SOUL.md / anti-patterns.md / evolution.jsonl                │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 4: 6-check validation                                  │
   │  4 content: stereotype / critique / refusal / trade-off      │
   │  2 structural: anchor consistency / family-specific check    │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 5: Delivery                                            │
   │  <sub-specialty-slug>/                                       │
   │    SOUL.md, SKILL.md, identity.json, evolution.jsonl,        │
   │    references/{capability-map, capability-clusters/,         │
   │                anti-patterns, critique-rubric}.md            │
   │  + GENERATION_REPORT.md (honest ledger + anti-pattern audit) │
   └──────────────────────────────────────────────────────────────┘
```

</details>

---

## Project structure

This repo is itself a project-level dogfood layout for Claude Code — all skills live under `.claude/skills/`, auto-discovered. Copy a subdirectory to `~/.claude/skills/` to install globally.

```
./
├── README.md                         # Chinese (default)
├── README.en.md                      # English
├── README.ja.md                      # Japanese
├── LICENSE                           # MIT
├── CHANGELOG.md                      # Version history
├── ARCHITECTURE_v0.2.md              # Architecture decision draft
└── .claude/
    └── skills/                       # Claude Code native skills path
        ├── INDEX.md                  # Registry of all distilled roles (v0.4)
        │
        ├── luban-skill/              # Meta-tool (generates new roles)
        │   ├── SKILL.md
        │   └── references/
        │       ├── generation-protocol.md     # Main flow (§0 7 stances + §1-§15)
        │       ├── domain-families.md         # 5-family skeleton
        │       ├── evolution-protocol.md      # Semi-auto evolution
        │       ├── seed-prospect-protocol.md  # Seed-prospect report format
        │       ├── identity-schema.json       # identity.json JSON Schema
        │       ├── soul-template.md           # Optional SOUL.md scaffold (9-section, v0.4)
        │       ├── skill-template.md          # Optional role SKILL.md scaffold
        │       ├── capability-map-template.md
        │       ├── critique-rubric-template.md
        │       └── anti-patterns-template.md
        │
        ├── agents/                   # /agents meta-skill (v0.4 — browse all roles)
        │   └── SKILL.md
        │
        └── infra-pm/                 # First distilled role: Mira the PM (v0.4)
            ├── SKILL.md
            ├── SOUL.md               # 9-section persona
            ├── identity.json
            ├── evolution.jsonl       # 3 entries v0.1→v0.3
            ├── GENERATION_REPORT.md  # Honest ledger
            └── references/
                ├── capability-map.md
                ├── capability-clusters.md
                ├── critique-rubric.md
                ├── anti-patterns.md
                ├── retrieval-sources.md
                ├── evolution-protocol.md
                ├── corpora-candidates.md
                └── source-material/  # Seed source archive
```

All `*-template.md` files are **optional scaffolds**, not mandatory templates.

---

## Why this project exists

[colleague-skill](https://github.com/titanwings/colleague-skill) showed that "distilling a specific person" works. [Nuwa](https://github.com/alchaincyf/nuwa-skill) pushed that to the limit — distilling Munger, Naval, Musk, real people with massive public corpora.

But most practitioners don't want a Musk conversation. They want a judge who can review their PRD like a senior B2B SaaS PM would. **That's not distilling a person; it's distilling a professional methodology.**

The hard part of distilling methodology is not "how to make the LLM play an expert" — that problem is known-solved, and the result is mediocre (see generation-protocol §0 stances 1-2 for the critique of vibes persona). The hard part is two things:

1. **How expertise forms**: from accumulated critique corpora (not blogs), standards docs (not descriptions), failure cases (not success stories)
2. **How expertise is verified**: by 6 checks all passing (4 content — stereotype / critique / refusal / trade-off, plus 2 structural — anchor consistency / family-specific check), not by "I feel it sounds right"

`luban-skill` engineers both into an executable protocol.

**luban builds tools. But the tools aren't pulled from thin air — they grow from craftsmen failing critique, accumulating standards, recording failures. luban-skill is the meta-tool for that process.**

---

## Status & Changelog

v0.3.0 has shipped — methodology internalization refactor complete; luban runs standalone. Full version history in [CHANGELOG.md](CHANGELOG.md). Architecture decision draft in [ARCHITECTURE_v0.2.md](ARCHITECTURE_v0.2.md).

---

## Acknowledgements

luban did not appear from nowhere. The works below each solved part of "how to engineer a role / persona / professional capability." luban stands on their shoulders and chose a different path (distilling sub-specialty methodology, not distilling individuals; no persona portability).

- **Persona thinking — [DeepPersona: A Generative Engine for Scaling Deep Synthetic Personas](https://arxiv.org/abs/2511.07338)** (Wang et al., 2025)
  Two-stage taxonomy-first + progressive specification framework. luban's Stage 1 "Capability Taxonomy Mining" → Stage 3 "Progressive Specification (5:3:2 sampling)" shares this lineage; the difference is luban refuses LLM-fabricated taxonomy and requires it to be reverse-engineered from critique corpora.

- **Distillation inspiration — [Nuwa-skill](https://github.com/alchaincyf/nuwa-skill)** (alchaincyf)
  Engineering-grade "distill a specific person"; proved distillation-as-skill viability. luban is its orthogonal complement: Nuwa distills individual mental models; luban distills sub-specialty methodology. Boundary documented in [SKILL.md §6](.claude/skills/luban-skill/SKILL.md).

- **SOUL structure — [soul-protocol](https://github.com/qbtrix/soul-protocol)** (qbtrix)
  Full spec for portable AI identity (metadata / OCEAN persona / 5-layer memory / state mgmt / .soul archive). luban's `SOUL.md` is a minimal subset — only persona / voice / stance — because luban's concern is professional judgment, not portability.

- **SOUL method — [OpenClaw `SOUL.md` concept](https://docs.openclaw.ai/concepts/soul)**
  "Where your agent's voice lives" — established SOUL.md as a standalone persona-layer file. luban adopts this positioning directly and strictly separates it from the capability layer (SKILL.md) and identity layer (identity.json).

If your work is related to luban and you want to be listed here, please open an issue.

---

## License

MIT
