# Karina Nguyen (OpenAI Research, ex-Anthropic) — eval-as-Schelling-point + agent UX as discipline

> Source A: "Things I learned at OpenAI" — https://semaphore.substack.com/p/things-i-learned-at-openai
> Source B: Latent Space "Agent Reasoning Interface" — https://www.latent.space/p/karina
> Fetched: 2026-05-30
> Distilled for: `ai-pm` (Mira) — Tier-2 enrichment to capability 3 (Eval pipeline design) + a new capability seed on agent-UX-as-discipline

## Core thesis 1: eval as strategic deliverable

**A great eval is a Schelling point that orients the field; designing the right eval is often higher leverage than building the model that scores on it.**

This recasts eval from QA-after-the-fact to **PM/research strategy artifact**:
- Eval definition fixes what "good" means → researchers across labs converge on it → progress accelerates on the dimensions you chose
- The framing question is no longer "how do we measure this?" but "what's the eval whose existence would most change the field?"
- Practitioner gap: the difference between "trying things" and "systematically narrowing hypotheses" — i.e., between vibes-driven iteration and hypothesis-driven evaluation

## Core thesis 2: agent UX is a distinct design discipline

Reasoning UX (o1/o3 chain-of-thought), canvas UX (collaborative editing), task UX (background async work), operator UX — each requires different surface contracts for:

- **Trust** — what should be shown vs hidden
- **Interruption** — when can the user step in
- **State** — how is partial progress represented

Designing the interface shapes what the model is trained to do, not the other way around. Surface design → training signal → model behavior.

## What this skill should now check for

- When user asks "how do we measure this agent": don't just answer the question. Ask: "Is this eval one that would orient others, or just yours?"
- When user designs an agent product surface: identify which of the four UX modes (reasoning / canvas / task / operator) it falls into; check whether trust / interruption / state contracts are explicit
- When user is in vibes-iteration mode ("let's try X and see"): translate to hypothesis form before proceeding

## Limits of this source

- Karina has access to model-training feedback loops that most PMs don't; "surface design → training" requires you have the lever
- "Schelling point" framing presumes you're playing at a level that influences the field; smaller scopes still benefit but the language is grand
- The four-UX-mode taxonomy is Karina's framing; should be treated as one cut, not the only cut

## Cross-link

- Pairs with `anthropic-krieger-research-coupling.md` — both treat product as downstream of model/research; Karina adds the eval-direction angle
- Pairs with `openai-lopopolo-harness-frame.md` — Lopopolo's "tests encode taste" is a sibling of Karina's "evals encode goal"
