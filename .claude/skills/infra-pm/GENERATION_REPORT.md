# GENERATION_REPORT — Agent Infrastructure PM (0→1 PMF)

> 诚实账单。luban v0.3.0 生成。本文件是用户判断"这个角色能不能用、什么时候不该用"的唯一依据。

---

## 1. 输入与路径

| 项 | 值 |
|---|---|
| 用户指令 | `/luban-skill 生成一个AI Agent产品经理的专家` |
| 域 | AI Agent PM |
| Sub-specialty (强制收敛) | Agent infrastructure PM at 0→1 PMF — runtime/orchestration/memory/tools/evals, dev-facing |
| **Display name** | **Mira the PM** (v0.2.0 起，per SOUL.md §1) |
| **Slash command** | **`/infra-pm`** (Claude Code 浮动面板可见，v0.3.0 起) |
| **In-text reference** | `@infra-pm` (仅文字引用，非 UI handle) |
| Domain family | product_growth_design + engineering |
| 阶段 | 0→1 PMF 探索 |
| 路径 | A+B 混合（用户选择）—— luban 用 web_fetch + web_search 抓取了核心种子 |
| Generation mode | **weak-seeded** |
| Vibes risk | **medium** |
| 生成时间 | 2026-05-27 (last upgrade 2026-05-28) |
| 版本 | **v0.3.0** (v0.2.0 → v0.3.0: 框架集成升级，路径迁移 + handle 语义修正 + first-encounter 规则) |
| 当前路径 | `.claude/skills/infra-pm/` (v0.3.0 起 — Claude Code 原生 skills 路径) |
| 旧路径 | `.claude/agent/agent-infra-pm/` (v0.2.0, 已迁移) |

---

## 2. 种子材料

### 已使用（archived under `references/source-material/`）

- **Anthropic — Building Effective Agents** (https://www.anthropic.com/engineering/building-effective-agents)
  - 类型: standards_doc（信号密度: 高）
  - 用途: workflow/agent taxonomy / ACI 设计原则 / framework 取舍立场 / "don't do" 直接引用
- **Senior AI Platform PM JD signals** (合成自 Anthropic careers、Greenhouse postings、Aakash Gupta 2026 playbook、Aman Khan/Arize 写作、AI Career Lab)
  - 类型: interview_bank（信号密度: 中-高）
  - 用途: capability 横切验证 / evals 作为 Tier-1 的依据 / 资深 PM 标准

### 缺失（影响 vibes_risk）

- 没有真正的 critique corpora 原文（如 LangChain/AutoGen maintainer reject 评论的批量样本）
- 没有公开的 0→1 阶段 Agent infra postmortem 长文
- 没有 production eval pipeline 的具体 case study（Hamel/Eugene Yan 文章未被 luban 完整 fetch，仅作为 retrieval-source 列出）

升级路径见 `references/corpora-candidates.md` 与 `references/anti-patterns.md` §4。

---

## 3. Validation 结果

### A 组 — 内容质量 4 check

| # | Check | 结果 | 关键证据 |
|---|---|---|---|
| 1 | Stereotype check | ✅ 通过 | 拒绝回答"AI Agent 怎么做才成功"，要求 user/task/metric disambiguation |
| 2 | Critique check | ✅ 通过 | 对中等 PRD 样本给出 5 条优先级合理的批评，对齐 BEA + JD 共识 |
| 3 | Refusal check | ✅ 通过 | 数据库多租户问题识别为 staff-engineer 让位条件 |
| 4 | Trade-off check | ✅ 通过 | LangChain vs SDK 问题反问 3 个 disambiguating 条件再给 stance |

### B 组 — 结构一致性 2 check

| # | Check | 结果 | 关键证据 |
|---|---|---|---|
| 5 | Anchor consistency | ✅ 通过 | 3 个不同主题（memory / eval / MCP）回答均能反推至少 2 个 anchor，无冲突 |
| 6 | Family-specific check | ✅ 通过 | Family 4 全部 5 项 + Family 2 全部 5 项检查项均嵌入 critique-rubric.md |

**总结**: 6/6 通过。

---

## 4. Honest limits 摘要

详细版见 `references/anti-patterns.md` 与 `identity.json` `honest_limits`。

1. weak-seeded 模式，11 条 capability 标 `[unverified]`，主要集中在 memory product surface / MCP commitment / eval calibration / 0→1 隐性 PM 判断
2. Sub-specialty 严格限定 0→1 PMF。扩张期决策应转介或新建 sibling role
3. Family 4 主属 + Family 2 检查项部分套用 —— 不能替代 staff engineer 架构权威 / AI safety researcher / 律师
4. 种子截止 2026-05-27，后续模型/SDK/标准更新需交叉验证

---

## 5. 下一步建议（如何升级到 seeded 模式 / vibes_risk: low）

按 `references/corpora-candidates.md` 第 1 类（critique corpora），抓取以下 2-3 项可显著提升：

1. **LangChain / AutoGen / CrewAI 主仓 issues 中 maintainer 的长 review comment**（5-10 条具体 issue）→ 消化 capability-map 中关于 framework 取舍 / abstraction 边界的 [unverified]
2. **Hamel Husain "Your AI product needs evals" 全文 + Eugene Yan eval 写作合集** → 消化关于 eval pipeline 的 3 条 [unverified]
3. **3-5 篇 "Why we replaced X" 长文** → 给 anti-patterns §2 提供具体 failure case 锚定

抓到后用 `evolution-protocol.md` 流程整合：
- `kind: seed_added` entry → 落进 `evolution.jsonl`
- 评估升级 `identity.json` 的 `generation_mode` / `vibes_risk`
- 删除被消化的 [unverified] 标记，更新 `capability-map.md` 末尾清单
- 在本报告添加"升级日志"段

---

## 6. 产物清单

```
.claude/skills/infra-pm/             (v0.3.0 起；旧路径 .claude/agent/agent-infra-pm/)
├── SOUL.md
├── SKILL.md
├── identity.json
├── evolution.jsonl                   (含 3 条 v0.3.0 升级 entry)
├── GENERATION_REPORT.md              (本文件)
└── references/
    ├── capability-map.md
    ├── capability-clusters.md
    ├── critique-rubric.md
    ├── anti-patterns.md
    ├── retrieval-sources.md
    ├── evolution-protocol.md
    ├── corpora-candidates.md
    └── source-material/
        ├── anthropic-building-effective-agents.md
        └── platform-pm-jd-signals.md
```

11 个文件 + 1 个空 jsonl + 1 个 source-material 子目录。

---

## 6a. v0.2.0 升级日志（persona-fication）

按 luban-skill v0.3.0 的新 SOUL 规范重做：

- **SOUL.md** 从 5-section 扩展到 9-section（增 §1 Identity / §2 Voice / §4 First-encounter intro / §5 How to work with me）
- **Display name** "Mira the PM" 经用户确认（nickname-the-role 模式），handle `@infra-pm`
- **anti-patterns.md** 新增 §0 "Persona-vs-Vibes 防漂移段"，列出 Mira 不可越界的虚构生平 / 履历 / 拟人化诱导
- **identity.json** version 0.1.0 → 0.2.0；不增加 persona 字段（per 用户决定，persona 单源在 SOUL.md）
- **6-check validation 再跑（v0.2.0）**:
  - A1 Stereotype: ✅ Mira 拒绝抽象问题（First-encounter intro 直接要求 disambiguation）
  - A2 Critique: ✅ critique 模式 Tone 直接，与 §3 一致
  - A3 Refusal: ✅ 让位条件保留（架构 / 法律 / safety / scale）
  - A4 Trade-off: ✅ 反问 + stance + 反驳 三段式
  - B5 Anchor consistency: ✅ Stance 5 条与 identity.json anchors 一一对齐
  - B6 Family check: ✅ Family 4 + 2 检查项保留在 critique-rubric.md
- **Persona-vs-vibes 新增 3 项**（v0.3.0 强制）:
  - ✅ Mira 没有过去 / 公司 / 年限（在 §8 No-go 显式列出）
  - ✅ 拟人化越界有 §9 + anti-patterns §0 双层防御
  - ✅ Drift 防御明示（50+ 轮重启建议）

---

## 6b. v0.3.0 升级日志（框架集成）

按用户反馈做了三件互相关联的修正：

1. **路径迁移**: `.claude/agent/agent-infra-pm/` → `.claude/skills/infra-pm/`
   - 原因：`.claude/agent/` 不是 Claude Code 原生扫描路径，skill 不可发现
   - 影响：现在 `/infra-pm` 出现在 Claude Code `/` 浮动面板，框架原生识别
   - did 字段同步更新：`expert:agent-infra-pm` → `expert:infra-pm`

2. **Handle 语义修正**: SOUL.md §1 从 "Handle: @infra-pm" 拆为两条
   - `Slash command: /infra-pm` (真调用入口)
   - `In-text reference: @infra-pm` (仅文字习惯，非 UI handle)
   - 原因：Claude Code 的 `@` 浮动面板只列文件，不列 skills

3. **First-encounter 规则收紧**: SOUL.md §4 加入强制触发规则
   - 仅首次显式召唤或新会话首次激活时使用一次
   - 后续轮次禁止自报家门 / 禁止加 prefix
   - 原因：用户观察到 Mira 每轮回答都加 "Mira。" prefix，违反 §7 brevity + 滑向表演性 persona

4. **生态配套**: 新建 `.claude/skills/INDEX.md` + `.claude/skills/agents/SKILL.md` meta-skill
   - INDEX.md 记录全部已蒸馏角色
   - `/agents` meta-skill 渲染结构化浮动面板（按 family 分组）
   - luban-skill 协议更新：未来生成角色自动 append 到 INDEX.md

完整变更记录见 `evolution.jsonl` (3 个 entry，全部 user_confirmed: true)。

---

## 7. luban 协议自省

本次生成遵循的关键决策点：

- §1.1 sub-specialty 强制收敛 → ✅ 通过 AskUserQuestion 收敛到 Agent 基础设施 PM (0→1)
- §3 zero-shot 前必产 corpora-candidates → ✅ 已产出，用户选 A+B
- §4 capability 叶节点必 actionable → ✅ 11 条 [unverified] 显式标记，未藏
- §10 identity.json schema → ✅ 符合 v0.2 schema 全部 required 字段
- §13 6-check 全跑 → ✅ 6/6 通过
- §15 协议本身的 honest limits → 承认：种子密度 ≠ 真专家在职经验密度，weak-seeded 的 medium vibes_risk 是真实评估，不是 false modesty
