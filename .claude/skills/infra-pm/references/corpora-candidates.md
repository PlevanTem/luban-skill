# Corpora Candidates: Agent 基础设施 PM (0→1 PMF 探索)

> Generated: 2026-05-27
> luban 没能从你的输入中识别到种子材料。下文是该 sub-specialty 的高信号种子源候选 —— 按信号密度排序（见 generation-protocol §0 立场 4）。
>
> 这个 sub-specialty 的特殊性：领域只有 ~3 年公开历史（2023-2026），critique corpora 数量比成熟领域少，但也意味着公开材料相对集中、噪声较小。

---

## 1. Critique corpora（最高信号）

- **LangChain / LlamaIndex / AutoGen / CrewAI 主仓 issues 与 PR review** — https://github.com/langchain-ai/langchain/issues + https://github.com/microsoft/autogen/issues
  - 为什么对此 sub-specialty 有信号: maintainer 在拒绝 feature / 关闭 issue 时会显式说明"为什么不该这么抽象"、"为什么 orchestration 应该简单"。是 Agent infra 设计决策的 critique 现场
  - 如何抓取: 过滤 closed issues + maintainer (如 hwchase17, microsoft 团队成员) 的长 comment；优先 2024 下半年至今（早期 issue 偏 bug，后期偏架构判断）
  - 大致体量: 大

- **Anthropic Engineering Blog — Building Effective Agents 系列** — https://www.anthropic.com/engineering/building-effective-agents 及关联文章
  - 为什么对此 sub-specialty 有信号: 直接陈述"哪些抽象不要做"（"don't build a framework before you understand the workflows"），是平台型 PM 最稀缺的 negative space
  - 如何抓取: 全文 + 引用的 cookbook 代码示例。配套读 Cookbook 仓库的 PR review: https://github.com/anthropics/anthropic-cookbook
  - 大致体量: 小（但密度极高）

- **Latent Space podcast transcripts**（涉及 Cursor / Replit / Devin / Anthropic / OpenAI Platform 团队的访谈） — https://www.latent.space/
  - 为什么对此 sub-specialty 有信号: 主持人 swyx 会追问"为什么砍了那个 feature"、"你们的 eval 怎么搭"，受访人通常是 infra team 的 staff/principal——是 0→1 Agent infra 决策的口述史
  - 如何抓取: 找标题含 "Cursor / Devin / agents / platform / eval" 的几期，读 transcript（站上有）
  - 大致体量: 中

- **"Why we ditched X" 类 retrospective 长文**（社区中持续出现）— 例如 Octomind "We replaced LangChain"、Hamel 多篇 eval-failure 案例
  - 为什么对此 sub-specialty 有信号: 替换 / 砍掉一个 infra 选择的理由就是 PM 应当听到的"用户为什么走了"
  - 如何抓取: Google "why we replaced langchain"、"langchain alternatives we tried" — 取 2024-2026 的，过滤纯营销贴
  - 大致体量: 中

- **MCP (Model Context Protocol) 规范 PR 讨论** — https://github.com/modelcontextprotocol/specification/pulls
  - 为什么对此 sub-specialty 有信号: MCP 是当前 Agent infra 最重要的标准化尝试，PR 中能看到 Anthropic / 微软 / 社区在权衡"接口要不要承诺这件事"——典型平台 PM 判断
  - 如何抓取: 过滤 review approve / changes-requested 的 PR，看 reviewer 的长评论
  - 大致体量: 中

## 2. 面试题库 / 评估清单（次高信号）

- **Anthropic / OpenAI / Cursor / Replit / Vercel AI 的 Platform PM / AI Product Manager 公开 JD** — 各公司 careers 页 + LinkedIn
  - 为什么对此 sub-specialty 有信号: senior 级别的 JD 显式列出 "must have" 能力（eval pipeline 设计、developer journey、cost-perf tradeoff），是该 sub-specialty 的能力评估清单
  - 如何抓取: 搜索 "AI Platform PM" / "Agent Platform PM" / "Developer PM AI" — 至少抓 5 份 senior+ 的 JD，对齐高频项
  - 大致体量: 小（每份短）

- **Reforge / Lenny's 的 AI/Agent PM 课程大纲（公开部分）** — https://www.reforge.com/, https://www.lennysnewsletter.com/
  - 为什么对此 sub-specialty 有信号: 即使付费内容不可见，章节标题与免费 preview 已能反映 senior PM 培训共识
  - 如何抓取: 抓课程页公开的 syllabus + outline
  - 大致体量: 小

- **"Your AI product needs evals" — Hamel Husain** + Eugene Yan eval 写作合集 — https://hamel.dev/blog/posts/evals/, https://eugeneyan.com/
  - 为什么对此 sub-specialty 有信号: 该 sub-specialty 最具差异化的能力是 eval pipeline 设计，这两位是公开写作中最系统的来源；可作为"能力评估"的客观参考
  - 如何抓取: 全文通读；重点抓 "eval 不该怎么做" 段落
  - 大致体量: 中

## 3. 标准 / 规范文档（高信号但单调）

- **Anthropic 官方 docs：Tool use / Computer use / Agents 章节** — https://docs.claude.com/en/docs/agents-and-tools
  - 如何抓取: tool use, multi-agent, claude code 三大块的 "best practices" 子页
  - 大致体量: 中

- **OpenAI "A Practical Guide to Building Agents" + Assistants/Responses API spec** — https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf
  - 大致体量: 小

- **MCP 规范本体** — https://modelcontextprotocol.io/specification
  - 为什么对此 sub-specialty 有信号: 当前 Agent infra 最重要的接口标准，规范文档反映"接口该承诺什么"
  - 大致体量: 中

- **Benchmark 设计文档：SWE-bench / τ-bench / GAIA / AgentBench / WebArena** — 各 paper + 仓库 README
  - 为什么对此 sub-specialty 有信号: benchmark 的"为什么这么设计 task"段落 = Agent 能力评估的方法论源头
  - 大致体量: 中

## 4. 失败案例集（中等信号）

- **Devin 发布后的复现失败 thread / Cognition 后续公开声明** — Twitter/X + Hacker News 2024-2025 archive
  - 如何抓取: 搜 "Devin reproduction" / "Devin demo" 的 thread；HN 高分讨论
  - 大致体量: 中

- **AutoGPT / BabyAGI 兴衰复盘文章** — 多篇 retrospective（如 Latent Space "AutoGPT obituary"）
  - 为什么对此 sub-specialty 有信号: 早期 Agent infra 的"通用 autonomy"路线为什么不 work，对 0→1 PMF 探索 PM 是反面教材
  - 大致体量: 中

- **LangChain 早期版本 breaking change 与社区怒吼归档** — GitHub issues + Reddit r/LangChain 历史帖
  - 为什么对此 sub-specialty 有信号: 0→1 阶段 infra 最容易踩的坑是过早承诺接口，这是公开样本
  - 大致体量: 大

## 5. 实践者博客 / 演讲（低信号，仅作补充）

- ⚠️ 警告: 博客是单一作者视角，容易锚定到他个人偏见。仅作多源交叉验证用，不要作为主种子。

- **Andrej Karpathy（X / YouTube 长讲） — agents/LLM OS 系列** — 单一愿景视角，但定锚 industry vocabulary
- **Lilian Weng — "LLM-powered Autonomous Agents"** — https://lilianweng.github.io/posts/2023-06-23-agent/ — 综述性，已被广泛引用为 vocabulary baseline
- **Simon Willison's Weblog** — https://simonwillison.net/ — 高频 + 实操，但单作者视角偏 hobbyist 与 ToC

---

## 用户选择

请回复以下之一：

- **A**. 我去抓 <列出你打算抓的资源 ID>，抓完回来 *（推荐路径）*
- **B**. 让 luban 用 web_search 抓 <最多 2 个资源 ID>
- **C**. 我接受 zero-shot 风险，直接生成（`vibes_risk: high` 会写入 identity.json）

如果你选 A，建议从第 1 类（critique corpora）开始抓——边际质量增益最大。最有性价比的两个：
1. Anthropic "Building Effective Agents" 全文（短、密度极高）
2. 5 份 senior Platform PM 的公开 JD（半小时可抓完，直接成为能力清单 cross-check）
