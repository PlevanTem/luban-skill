# Capability Map: Agent 基础设施 PM (0→1 PMF)

> Sub-specialty: Agent infrastructure PM at 0→1 PMF exploration (runtime / orchestration / memory / tools / evals)
> 数据源密度: weak-seeded（Anthropic BEA + 公开 senior PM JD + 行业共识）
> 总叶节点数: 58 条 | [unverified] 节点数: 11 条

来源 ID 映射：
- `bea` = `source-material/anthropic-building-effective-agents.md`
- `jd` = `source-material/platform-pm-jd-signals.md`
- `unv` = 无明确种子追溯，标 [unverified]

---

## 1. Domain knowledge（Agent 系统的工作原理）

### 1.1 模式与组件
- Can name the 5 workflow patterns (prompt chaining / routing / parallelization / orchestrator-workers / evaluator-optimizer) and the agent pattern, and articulate when each applies [来源: bea]
- Knows when "agent" is the wrong abstraction—open-ended, many-step, sandbox-able problems only; everything else is a workflow [来源: bea]
- Can decompose a user task into "what code paths should be predefined" vs "what should the LLM decide at runtime" [来源: bea]

### 1.2 Tool / ACI design
- Treats agent-computer interface (ACI) design with the same rigor as human-computer interface design [来源: bea]
- Can audit a tool schema for cognitive load: ambiguous parameters, missing examples, format friction (escape chars, line counting) [来源: bea]
- Knows to test "many example inputs" before shipping a new tool, and to apply poka-yoke (mistake-proofing) — e.g., absolute paths over relative [来源: bea]

### 1.3 Memory / context / state
- Can articulate the difference between session memory, long-lived agent memory, retrieval-grounded context, and tool-state—and which 0→1 scenarios actually need persistent memory [来源: jd]
- Knows that memory is a *product* surface, not a *technical* surface: retention/forgetting policy is a UX decision [unverified]

### 1.4 MCP / interop standards
- [unverified] Tracks MCP and similar interop specs; can articulate which capabilities to commit-to vs leave open [来源: unv]

---

## 2. Methods & frameworks（0→1 工作方法）

### 2.1 假设驱动的 0→1
- Can write a "smallest testable assumption" for an agent product (e.g., "users will accept agent action X if confidence ≥ Y, evaluated on dataset Z") [来源: jd]
- Knows the 0→1 trap: shipping the framework before understanding the workflow [来源: bea]
- Can run a ship-to-learn cycle weekly, not quarterly [来源: jd]

### 2.2 Eval pipeline 设计（核心方法论）
- Can design an offline eval set from scratch: task definition → golden trajectories → automated scorer → manual spot-check rubric [来源: jd]
- Can distinguish between eval that measures *capability* (does the model can do it) vs eval that measures *product* (does the user succeed) [来源: jd]
- Knows when LLM-as-judge is appropriate and when it's not (and how to calibrate it against human raters) [unverified]
- Can build a regression gate: "this PR cannot land if eval set X drops > Y%" [来源: jd, bea]
- Treats evals as a versioned artifact, not a one-off spreadsheet [来源: jd]

### 2.3 选型与对比
- Can compare two implementations (e.g., "single-shot LLM" vs "evaluator-optimizer loop") on cost, latency, quality, and debuggability—not just quality [来源: bea]
- Refuses to adopt a framework before understanding the underlying primitives [来源: bea]
- Knows when "boring" wins: a deterministic workflow that solves 80% beats an agent that solves 95% but fails opaquely [来源: bea]

### 2.4 Customer co-development（0→1 必备）
- Can run a design partner program: 3-5 customers, weekly cadence, written debriefs [unverified]
- Knows the difference between "what the customer says" and "what the customer's agent logs show" [来源: jd, bea]

---

## 3. Tools & artifacts（产出物）

### 3.1 写作产出
- Writes a one-page PRD that leads with assumption + eval criteria, not with feature list [来源: jd]
- Writes tool/skill specs that include: docstring, examples, edge cases, failure modes, boundary with other tools [来源: bea]
- Writes weekly investor/team updates that lead with "eval delta this week", not "features shipped" [unverified]

### 3.2 原型 & 数据
- Can prototype in code (Python / TS) with direct LLM API calls — not waiting for engineering [来源: bea, jd]
- Can build and maintain an eval harness (custom script or Inspect/Promptfoo/Braintrust/LangSmith) [来源: jd]
- Can run a dogfooding instance and surface its logs to the team [unverified]

### 3.3 Decision records
- Maintains a decision log for irreversible choices: framework adoption, prompt structure, tool contract [来源: bea]

---

## 4. Judgment & taste（无法 codify 的判断）

### 4.1 复杂度判断
- Can detect over-abstraction within minutes of reading a teammate's design doc [来源: bea]
- Will remove a tool / step / agent layer when measurement does not justify it [来源: bea]
- Knows that *transparency* (showing planning steps) is often more valuable than higher single-shot accuracy [来源: bea]

### 4.2 反 hype 偏好
- Defaults to "use direct API calls" before considering any framework [来源: bea]
- Skeptical of demo-quality results that haven't been re-run on a held-out eval [来源: jd, bea]
- Distinguishes between "the model can do this in a notebook" and "users can do this in production" [来源: jd]

### 4.3 0→1 节奏感
- Knows when to *not* generalize—a working pattern for one customer is a finding, not a feature [unverified]
- Recognizes when the team is in "build mode" vs "learn mode" and protects the right one [unverified]

---

## 5. Critique standards（评审标准 — 信号密度最高的分支）

### 5.1 PRD / spec critique
- "Is the assumption falsifiable, and how would we know within 2 weeks?"
- "What's the eval that would tell us this is wrong?"
- "What's the simplest possible version? (Is there one we already rejected too fast?)"
- "Are the tool schemas readable by a new engineer in 2 minutes without context?"
- All four sourced from [bea, jd] synthesis

### 5.2 Architecture / design critique
- "Why is this an agent, not a workflow?" [来源: bea]
- "What's the latency / cost / failure-mode budget? Have we measured against it?" [来源: bea]
- "If this framework disappeared, could we still ship in 1 week?" [来源: bea]

### 5.3 Eval critique
- "Does this eval reflect production traffic distribution, or a convenience sample?" [unverified]
- "What's the inter-rater agreement on the human-labeled subset?" [unverified]
- "If the model improves on this eval, does the user metric also improve?" [来源: jd]

### 5.4 Launch readiness critique
- "What's the regression alarm—who gets paged on what threshold?" [来源: jd]
- "What's the rollback path if the agent misbehaves at scale?" [unverified]

---

## 6. Collaboration & communication

### 6.1 与研究/模型团队
- Can read a model card and translate capability deltas to product implications [来源: jd]
- Knows what to bring to a research conversation: data, not opinions [来源: jd]

### 6.2 与工程团队
- Will own the prompt + eval artifacts directly, not delegate them entirely to engineers [来源: jd]
- Distinguishes between "prompt is a config" (PM owns) and "infrastructure is code" (eng owns)—and writes the contract between them [unverified]

### 6.3 与 GTM / customer-facing
- Can co-author a customer-facing eval report (what works, what doesn't, what's coming) [unverified]

---

## 7. Industry & business context

### 7.1 价值捕获模式
- Knows the existing pricing patterns: per-token, per-resolution (Klarna/Sierra-style), seat-based, hybrid—and when each applies to an agent product [来源: bea]
- Can argue when *not* to monetize before PMF (and counter-argue when monetization itself is the learning signal) [unverified]

### 7.2 Buyer / user 分离
- Recognizes that platform Agent products often have separate buyer (CTO/eng leader) and user (developer); writes for both [来源: jd]

---

## 8. Ethics & failure modes

### 8.1 已知 0→1 失败模式
- Premature framework abstraction—building a "platform" before understanding the workflows [来源: bea]
- Demo-driven roadmaps—shipping what looks good in a video, not what works on the held-out eval [来源: jd, bea]
- Eval theater—publishing a benchmark number that doesn't move the user metric [来源: jd]
- Agent autonomy without sandbox / reversibility [来源: bea]
- Generalizing from 1 customer's working pattern [unverified]

### 8.2 安全 / 信任边界
- Knows when a use case requires confirmation gates vs full autonomy [来源: bea]
- Aware that "fully autonomous" is a higher product bar than the team usually estimates [来源: bea]

---

## [unverified] 节点清单

11 条标 [unverified] 的叶节点（无明确种子追溯，建议在 evolution 阶段补种子升级）：

1. 1.3 / Memory as product surface — 行业共识但无直接 critique corpora
2. 1.4 / MCP capability commitment — MCP 仍在快速演进，立场不稳
3. 2.2 / LLM-as-judge calibration — 方法论存在但公开 critique 稀少
4. 2.4 / Design partner program cadence — PM 经验性知识，无标准化来源
5. 3.1 / Weekly update with eval delta — 行业实践但无公开文献
6. 3.2 / Dogfooding log surfacing — 同上
7. 4.3 / Build mode vs learn mode — 隐性 0→1 PM 判断
8. 4.3 / 1 customer = finding, not feature — 同上
9. 5.3 / Inter-rater agreement on eval — 标准方法但需补 NLP/ML eval 文献
10. 5.4 / Rollback path for misbehaving agent — 运维实践，需补 incident retrospective
11. 6.2 / Prompt-as-config vs infra-as-code — 团队组织实践

升级路径见 `corpora-candidates.md` —— 抓 LangChain/AutoGen GitHub issue archive + Hamel/Eugene Yan eval 文章合集，可消化掉这 11 条中的 7-8 条。

---

## v0.4 enrichment: OpenAI / Anthropic PM thought leadership

v0.4 增补 5 个 practitioner 思想 leadership 源（详见 `source-material/`），新增以下 Tier-2/3 子能力：

**进 capability 2 (Tool/ACI schema & harness critique)**:
- 2.x **Harness 而非代码是稀缺资源** [Lopopolo] — 检查 user 是否把品味 encode 进 tests 而非 Slack 论证
- 2.x **Painted-door 设计** [Lopopolo] — 前端先做完，backend 看数据决定是否实现；明确拒绝"先 backend 再 UI"

**进 capability 3 (Eval pipeline design)**:
- 3.x **Eval as Schelling point** [Karina Nguyen] — eval 是 strategic deliverable，问 "这 eval 能 orient 别人吗"，不是 QA 后置

**进 capability 4 (PRD critique)**:
- 4.x **Ship-to-understand** [Turley] — AI 产品 PMF 信号 post-launch 才浮出；PRD 是 launch lever 不是 pre-launch validator

**进 capability 6 (Customer co-dev)**:
- 6.x **非常规早期信号渠道** [Turley] — ChatGPT 早期 monitor TikTok 评论；问 user 他们的 AI 产品等价渠道是什么
- 6.x **Prototype : ship ≥ 5:1** [Cherny] — Claude Code 给出的探索密度 benchmark；< 5:1 说明探索不足

**进 capability 8 (Research-coupling)** — 从原"协作"升级到"耦合"：
- 8.x **PM-research 距离即杠杆** [Krieger] — UX-on-top-of-models 的 PM 约 1/10 leverage vs co-located-with-post-training
- 8.x **3 个 PMF 信号** [Krieger] — task-level success metric + 强日活 + **系统在模型升级时自动改善**（第 3 个是 moat test）

**新增 agent UX 设计子分支**（Tier-3）:
- **Reasoning / canvas / task / operator 四类 agent UX** [Karina Nguyen] — 每类对 trust / interruption / state 的 surface contract 不同；不要混用

所有上述子能力都已加 source 引用，可在 `source-material/` 对应文件追溯。

