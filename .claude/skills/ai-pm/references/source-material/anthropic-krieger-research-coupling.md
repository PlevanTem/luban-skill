# Mike Krieger (Anthropic CPO) — research-coupling + PMF signals

> Source A: Lenny's Podcast "Anthropic's CPO on what comes next" — https://www.lennysnewsletter.com/p/anthropics-cpo-heres-what-comes-next
> Source B: Sequoia Training Data "Building AI Products From the Bottom Up" — https://inferencebysequoia.substack.com/p/anthropic-cpo-mike-krieger-building
> Fetched: 2026-05-30
> Distilled for: `ai-pm` (Mira) — Tier-2 enrichment to capability 8 (Research-coupling)

## Core thesis 1: research-coupling

AI product planning must invert from traditional top-down 3-6mo roadmaps to **bottoms-up creativity built close to the model**. You can't tell what's possible until late. Embed product teams *inside* research, not adjacent to it.

- Concrete: **Artifacts and MCP both emerged from prototypes**, not roadmaps
- **PMs working on UX-on-top-of-models deliver ~10x less value than PMs co-located with post-training**
- Implication for org design: a PM whose nearest research counterpart is 2 levels away in the org is leaking the majority of their potential leverage

## Core thesis 2: three PMF signals for agent products

Distinguishes real AI PMF from vanity engagement:

1. **Clear task-level success metric** (resolution time, workflow length, etc.) — not just sessions or DAU
2. **Strong daily adoption** — not weekly, not "tried once and impressed"
3. **System that auto-improves when the model improves** ← the **moat test**

#3 is the differentiator: if your product gets better when Claude 4.6 → 4.7 lands without code changes, you have a real wrapper-resistance moat. If it doesn't, you're building a feature, not a product.

## What this skill should now check for

- When evaluating org structure for an AI team: ask how close PMs are to post-training. Flag if ≥2 reporting layers away.
- When user asks "do we have PMF": apply 3-signal test, especially the auto-improve test
- When user has top-down annual roadmap: push back; AI product roadmaps decay fast under model capability shifts

## Limits of this source

- Krieger's frame assumes you have *access* to model researchers (Anthropic insider view). For founders consuming model API, "research-coupling" needs reinterpretation as "model-eval-coupling" or "model-release-tracking discipline"
- The 10x claim is a single CPO's observation; treat as directional, not measured

## Cross-link

- Pairs with `karina-nguyen-eval-strategy.md` — both argue from the position that the product layer is *downstream* of model and eval design
- Counters generic startup advice ("annual planning", "stakeholder alignment") that assumes a stable substrate
