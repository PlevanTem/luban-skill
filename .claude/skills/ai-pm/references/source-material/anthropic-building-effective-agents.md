# Anthropic — Building Effective Agents (excerpted)

> Source: https://www.anthropic.com/engineering/building-effective-agents
> Fetched: 2026-05-27 by luban (web_search/web_fetch path B)
> Role: **Primary critique corpora** for Agent 基础设施 PM (0→1 PMF)

## Core thesis
The most successful LLM agent implementations use **simple, composable patterns rather than complex frameworks**. Success means building the *right* system for your needs, not the most sophisticated one.

## Three core design principles
1. **Simplicity** — maintain simplicity in agent design
2. **Transparency** — explicitly show the agent's planning steps
3. **Tool documentation & testing** — carefully craft agent-computer interfaces (ACI)

## Explicit "don't do" statements (high-signal for critique rubric)
- "Only increasing complexity when needed" / "add complexity *only* when it demonstrably improves outcomes"
- Frameworks "often create extra layers of abstraction that can obscure the underlying prompts and responses, making them harder to debug"
- "Incorrect assumptions about what's under the hood are a common source of customer error"
- "Start by using LLM APIs directly; many patterns can be implemented in a few lines of code"
- "Agentic systems often trade latency and cost for better task performance" — only justified when measurement shows improvement
- "If you do use a framework, ensure you understand the underlying code"

## Workflow vs Agent taxonomy
**Workflows** (predefined code paths): prompt chaining · routing · parallelization (sectioning/voting) · orchestrator-workers · evaluator-optimizer
**Agents** (LLM directs own process): autonomous, needs clear evaluation criteria + environmental feedback loops

## Decision matrix
| Pattern | When | Requirement |
|---|---|---|
| Simple prompt | Default | Measure first |
| Prompt chaining | Fixed, decomposable | Specialization improves accuracy |
| Routing | Distinct categories | Accurate classification possible |
| Parallelization | Speed/confidence gains | Independent subtasks |
| Orchestrator-workers | Unpredictable subtask count | Dynamic breakdown |
| Evaluator-optimizer | Clear eval criteria | Iterative refinement helps |
| Agents | Open-ended, many-step | Trust + sandboxed env |

## Tool / ACI design principles
- Treat ACI with same rigor as HCI
- Minimize cognitive load (is it obvious how to use this tool from the description?)
- Format close to natural text; avoid escaping / line-counting overhead
- Include examples, edge cases, input format, boundaries from other tools
- Poka-yoke (mistake-proofing)
- Iterative testing on many example inputs
- Concrete example: SWE-bench agent required **absolute filepaths**, not relative — eliminated common error class

## Customer use cases (proven)
- **Customer support**: conversation + action, integration with tools, clear success metrics, usage-based pricing (only pay for resolutions) signals provider confidence
- **Coding agents**: verifiability via tests, well-defined problem space, objective quality measurement

## Quotable lines for SOUL / anti-patterns
- "Success in the LLM space isn't about building the most sophisticated system."
- "Frameworks... can also make it tempting to add complexity when a simpler setup would suffice."
