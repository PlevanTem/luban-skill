# Boris Cherny (Claude Code lead, Anthropic) — prototype-density + on-distribution tech stack

> Source: Pragmatic Engineer "How Claude Code is built" (Gergely Orosz interviewing Boris Cherny)
> URL: https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built
> Fetched: 2026-05-30
> Distilled for: `ai-pm` (Mira) — Tier-2 enrichment to capability 6 (Customer co-dev / prototype density benchmark)

## Core thesis

For agent infrastructure products at 0→1, the leading PMF indicator is **prototype-to-ship ratio**, not user-facing metrics. If you're not throwing away 80%+ of what you prototype, you're not exploring enough.

## Concrete benchmarks (from Claude Code build process)

1. **10+ prototypes per shipped feature**
2. **~5 releases per engineer per day**
3. **~90% of the codebase written by the agent itself** (Claude Code building Claude Code — dogfood as forcing function)
4. **Tech stack chosen to be on-distribution for the model** — meaning languages, frameworks, idioms that the model has seen heavily in training. Default to popular > novel.

## Operating principles

- Prototype density is the leading indicator; downstream metrics (DAU, retention) lag and may not move for weeks even when the product is converging
- On-distribution choice is asymmetric: gain from being on-distribution is large (agent fluent), cost is small (using popular tech is rarely a real constraint at 0→1)
- Releases-per-engineer-per-day is a *team-health* metric — if it drops below 1/day, something is blocking the team (often: missing harness layer, per Lopopolo)

## What this skill should now check for

- When user reports "we shipped 3 features last month": ask the prototype:ship ratio. Under 5:1 means under-exploring.
- When user picks tech stack: ask "is this on-distribution for the model we'll use?" If exotic, the agent will be slower and weirder than necessary
- When team cadence < 1 release/eng/day: ask what specifically blocks — usually missing test harness, missing deployment pipeline, or PM-engineering handoff friction
- When dogfood opportunity exists: heavily favor it (Claude Code building Claude Code is the canonical example)

## Limits of this source

- These numbers are from Anthropic-internal build with model and infra teams co-located; small teams may not achieve 5 releases/eng/day even at best
- "On-distribution" stays right as a heuristic, but specific languages (Python, TypeScript, etc.) shift as models update — re-check what's actually on-distribution every 6mo
- 90% agent-written codebase is exceptional and assumes both strong harness and senior eng team — not a target for most orgs

## Cross-link

- Closely pairs with `openai-lopopolo-harness-frame.md` — Cherny gives the *what* (prototype density), Lopopolo gives the *how* (harness that enables it)
- Pairs with `openai-turley-ship-to-understand.md` — Cherny's prototype density is the engineering implementation of Turley's "maximally accelerated"
- Pairs with `anthropic-building-effective-agents.md` — patterns from BEA are what you prototype 10× against
