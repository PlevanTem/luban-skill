# Retrieval Sources

> 本角色在工作中会真实 reach for 的外部资源。Tier-3，按需检索。
>
> Source-of-truth：URL / 仓库 / 命令。不允许写"行业最佳实践"这种空指针。

---

## 1. 设计哲学 / 决策参考

- **Anthropic — Building Effective Agents** — https://www.anthropic.com/engineering/building-effective-agents
  - 何时调用: workflow-vs-agent 判断、framework 取舍、tool/ACI 设计 critique
- **Anthropic — How we build effective agents (extended cookbook code)** — https://github.com/anthropics/anthropic-cookbook
  - 何时调用: 用户问"workflow pattern 怎么实现"时直接给代码引用
- **OpenAI — A Practical Guide to Building Agents** — https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf
  - 何时调用: 与 Anthropic 立场对照、给出"两家厂商怎么说"

## 2. 接口标准

- **Model Context Protocol 规范** — https://modelcontextprotocol.io/specification
  - 何时调用: tool/skill 接口设计、外部系统集成、跨厂商可移植性
- **Anthropic Tool Use docs** — https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview
- **OpenAI Responses API / Assistants API spec** — https://platform.openai.com/docs/api-reference

## 3. Eval / benchmark 参考

- **SWE-bench Verified** — https://www.swebench.com/
  - 何时调用: coding agent 任务定义、verifiability 论证
- **τ-bench (TauBench)** — Sierra / Anthropic 联合
  - 何时调用: customer-service agent task design
- **GAIA** — General AI Assistants benchmark
- **AgentBench / WebArena** — 多 task agent 评估
- **Inspect (Anthropic eval harness)** — https://github.com/UKGovernmentBEIS/inspect_ai
- **Promptfoo / Braintrust / LangSmith** — production eval tooling

## 4. 实践写作（多源交叉验证用，不作单点权威）

- **Hamel Husain — "Your AI product needs evals"** — https://hamel.dev/blog/posts/evals/
- **Eugene Yan — eval writing 合集** — https://eugeneyan.com/writing/
- **Lilian Weng — "LLM-powered Autonomous Agents"** — https://lilianweng.github.io/posts/2023-06-23-agent/
- **Simon Willison's Weblog — Agents tag** — https://simonwillison.net/tags/agents/
- **Latent Space podcast** — https://www.latent.space/ — Cursor / Devin / Anthropic 平台团队访谈

## 5. 失败案例 / 反面教材

- **"Why we ditched LangChain" 类 retrospective** — Google 搜，2024-2026 archive
- **AutoGPT / BabyAGI 兴衰 retrospective**
- **Devin demo 复现讨论 thread** — HN + X 2024-2025 archive
- **LangChain 早期 breaking change issue archive** — https://github.com/langchain-ai/langchain/issues?q=is%3Aissue+breaking

## 6. 工具栈

实际 prototyping 与产出物会用到的：

- Python / TypeScript + anthropic-sdk / openai-sdk
- jupyter / marimo — eval 编辑与回放
- Linear / Notion / GitHub Issues — decision record / PRD 落盘
- Figma — 仅做 UX flow 草图，不做视觉

---

## 检索礼仪

- 引用 URL 时，附"何时调用"说明，不要裸贴
- 失败案例类（第 5 节）至少与第 1 节交叉验证，避免单点偏见
- 第 4 节（博客）是补充信号，不作单点裁决依据
