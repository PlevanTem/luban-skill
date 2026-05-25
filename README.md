# 鲁班.skill (luban-skill)

> Nuwa 蒸馏人，鲁班蒸馏专业方法论。

**把 "AI 扮演专家" 升级为 "AI 真的懂这个专业"。**

鲁班帮你把任何一门手艺——B2B SaaS 产品经理、刑事辩护律师、并购财务顾问、UX 设计总监——蒸馏成一个 Claude Code Skill，让 AI 像在这行干了十年一样跟你对话：会挑刺、会拒绝、会讲 trade-off，而不是给你一段 LinkedIn 简介式的人设。

[![Version: v0.3.0](https://img.shields.io/badge/version-v0.3.0-green)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)]()
[![Skill: Claude Code](https://img.shields.io/badge/skill-Claude%20Code-orange)]()

---

## 效果对比

> 同样一句：「帮我 review 这份 B2B SaaS PRD」

**❌ 普通 prompt / "扮演资深 PM" 人设**
> *[占位：v0.4 端到端示例补齐后填入真实输出 —— 典型表现是泛泛而谈、套话、给出"看起来对"的建议，但抓不住该 sub-specialty 的真问题]*

**✅ 用鲁班蒸馏的 `b2b-saas-pm` Skill**
> *[占位：v0.4 端到端示例补齐后填入真实输出 —— 典型表现是先质疑 ICP 假设、要求看 ARR 拆解、对接口设计的 multi-tenant 影响逐条挑刺、明确拒绝某些场景说"这超出我判断范围"]*

差距不来自"更好的 prompt"，来自鲁班把**该专业的 critique 标准、能力清单、决策启发法、自检 rubric** 都蒸馏进去了——详见 [工作原理](#工作原理)。

---

## 快速开始

1. **安装 skill** —— 本仓库本身就是 dogfood 布局：进入本仓库时，Claude Code 会自动从 `.claude/skills/luban-skill/` 加载。要装到全局自用，把 `.claude/skills/luban-skill/` 整目录复制到 `~/.claude/skills/luban-skill/` 即可。
2. **在 Claude Code 里召唤鲁班**（必须显式说出 "luban" 或 "鲁班"，不会被泛触发劫持）：

   ```
   用鲁班蒸馏一个 B2B SaaS PM 角色，我已经有 30 份 design review 实录
   ```

3. **没有种子材料？** 直接说：

   ```
   用鲁班帮我蒸馏一个刑事辩护律师，我手头什么都没有
   ```

   鲁班会进入**种子勘探模式**，给你一份"该 sub-specialty 的 critique corpora 候选清单"——告诉你去找什么、在哪找、按什么顺序找。

4. **产物**：一个完整的角色目录（`SOUL.md` + `SKILL.md` + `identity.json` + capability map + critique rubric + anti-patterns + 诚实账单 `GENERATION_REPORT.md`）。装回 Claude Code 直接用。

---

## 鲁班能为你做什么

- **你写 PRD**，想要一个真懂 B2B SaaS 的 PM 来挑刺，而不是 ChatGPT 套话。
- **你做合规自查**，想要一个真做过这行的律师按 rubric 走，而不是"假装是律师"的 prompt。
- **你设计组件库**，想要一个看过 500 个设计稿的 design director 给你做 critique，而不是"建议增加易用性"这种废话。
- **你写商业计划书**，想找见过 200 个项目的投资人按真实标准挑问题。
- **你做内部知识沉淀**，想把团队里某个 sub-specialty 的判断方法固化下来，而不是依赖某个具体的人还在不在。

鲁班不取代该专业的人，**它把"判断该专业活儿做得好不好的标准"工程化为可执行的 Skill**。

---

## 已蒸馏的 Skill 示例

| Sub-specialty | 状态 | 种子类型 |
|---|---|---|
| B2B SaaS Design Director | ⏸ v0.4 计划 | design review 实录 + 内部 rubric |
| *[更多示例待社区贡献]* | — | — |

> v0.3.0 ship 的是**元工具**（蒸馏方法论本身）。端到端示例在 v0.4，欢迎 PR 贡献你蒸馏出来的 sub-specialty。

---

## 和现有方案有什么不同

| 方案 | 它解决什么 | 鲁班不一样的地方 |
|---|---|---|
| **普通 prompt / "扮演资深 X"** | 让 LLM 看起来像专家 | 鲁班拒绝 vibes persona——不是描述一个专家，是按方法论蒸馏 |
| **[Nuwa](https://github.com/alchaincyf/nuwa-skill)** | 蒸馏具体真人 (Munger / Naval / Musk) 的 mental model | 鲁班不绑真人，蒸馏的是**该 sub-specialty 的方法论本身** |
| **[OpenPersona](https://github.com/acnlabs/OpenPersona)** | persona 生命周期管理（生成、约束、演化） | 鲁班关心的是"专业判断怎么形成"，不是 persona 怎么 portable |
| **soul.md 系列** (clawsouls / rokoss21 / aaronjmars) | AI agent 人格 portability | 同上，鲁班正交于人格层 |
| **RAG / 向量库** | 给 LLM 外挂领域知识 | 鲁班蒸馏的是**判断标准 + 决策启发法 + 自检 rubric**，不是文档检索 |

护城河在两件事：
1. **强制 sub-specialty**——不接受"产品经理"这种泛输入，必须收敛到"B2B SaaS PM"级别
2. **种子勘探模式**——用户没种子时，鲁班主动产出"该 sub-specialty 的 critique corpora 候选清单"，让用户去找

---

## 工作原理

### luban 的 7 条核心立场

完整定义在 [`.claude/skills/luban-skill/references/generation-protocol.md` §0](.claude/skills/luban-skill/references/generation-protocol.md)。摘要：

1. **拒绝 vibes persona**：禁止 "You are a world-class designer with 20 years of experience" 这类描述性 prompt——产出像 LinkedIn 简介，没有真判断。
2. **拒绝 LLM 凭空生成**：禁止让 LLM "describe an expert X" 来生成 capability——这是 stereotype 再生产，是大多数 "AI 专家角色" 项目的根本失败。
3. **强制 sub-specialty**："senior designer" 太宽，必须拆到 "B2B SaaS UX designer" 级别。
4. **数据源按信号密度排序**：critique corpora > 面试题库 > 标准文档 > 失败案例 > 实践者博客。
5. **三层加载**（Tier-1 always loaded / Tier-2 per task family / Tier-3 retrieval on demand）——不是把所有能力塞进 SKILL.md，而是按使用频率分层。
6. **5:3:2 progressive sampling**：在 Tier-1 内部选 5 条核心、3 条邻接、2 条远端能力——对抗 LLM 的刻板印象再生产。
7. **6-check validation 非选项**：内容质量 4 + 结构一致性 2，全过才算交付。

**最容易引起分歧的是立场 2 和立场 7**——这两条把"快速生成体验"和"严肃方法论"放在了对立面。鲁班选择了后者。如果你的产品愿景是前者，鲁班不适合你。

### 方法论血统

立场 1-7 的概念骨架来自 expert-persona-synthesis（Anthropic 内部研究方向）的吸收消化，加上鲁班在族骨架、SOUL 层、evolution 协议、种子勘探模式上的扩展。v0.3.0 起方法论已**内化为鲁班自身**——不再要求用户先安装 expert-persona-synthesis，鲁班独立运行。

### 蒸馏流程（5 阶段流水线）

```
输入领域 + sub-specialty + 种子
    ↓
[种子门槛检查] —— 零种子则进入勘探模式
    ↓
[Step 1] 领域族判定 → domain-families.md (5 族骨架)
    ↓
[Step 2] 5 Stage Pipeline:
    1. Capability Taxonomy Mining       → capability-map.md
    2. Anchor                           → identity.json
    3. Progressive Specification (5:3:2)→ SKILL.md + clusters
    4. Critique Rubric                  → critique-rubric.md
    5. Tools & Workflow                 → 嵌入 SKILL.md
    ↓
[Step 3] 人格层 + 诚实账单 → SOUL.md + anti-patterns.md + evolution.jsonl
    ↓
[Step 4] 6 check validation (内容 4 + 结构 2)
    ↓
[Step 5] 交付目录 + GENERATION_REPORT.md
```

<details>
<summary>展开完整架构图</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                          用户输入                                    │
│  领域 (e.g. PM)  +  Sub-specialty (e.g. B2B SaaS PM)  +  种子材料   │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
              ┌───────────────────────────────────┐
              │  种子门槛检查（generation-protocol §1）│
              └───────────────────────────────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              ▼                                       ▼
     ┌─────────────────┐                  ┌──────────────────────┐
     │  有 ≥1 项种子    │                  │  零种子               │
     │  → 进入生成流程  │                  │  → 种子勘探模式       │
     └─────────────────┘                  │  → SEED_PROSPECT.md  │
              │                            └──────────────────────┘
              │                                       │
              │                                       ▼
              │                            ┌──────────────────────┐
              │                            │ 用户找到种子后回来    │
              │                            └──────────────────────┘
              │                                       │
              ▼                                       │
   ┌──────────────────────────────────┐              │
   │  Step 1: 领域族判定               │◀─────────────┘
   │  → domain-families.md            │
   │  → 5 个族 × 6-12 个 sub-specialty │
   └──────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 2: luban 5 Stage Pipeline 执行                          │
   │  ┌──────────────────────────────────────────────────────┐   │
   │  │ Stage 1: Capability Taxonomy Mining                  │   │
   │  │  数据源信号密度: critique > 面试题 > 标准 > 失败 > 博客│   │
   │  │  → capability-map.md (3 层 markdown 树)              │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 2: Anchor                                      │   │
   │  │  → identity.json (sub-specialty, philosophy, 约束)    │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 3: Progressive Specification (Tier 1/2/3)      │   │
   │  │  5:3:2 sampling: 5 核心 + 3 相邻 + 2 远缘             │   │
   │  │  → SKILL.md (Tier 1) + capability-clusters/ (Tier 2) │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 4: Critique Rubric                             │   │
   │  │  通用层 + 族特定层 + sub-specialty 层                 │   │
   │  │  → critique-rubric.md                                │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │ Stage 5: Tools & Workflow                            │   │
   │  │  → 嵌入 SKILL.md 的 workflow section                  │   │
   │  └──────────────────────────────────────────────────────┘   │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 3: 人格层 + 诚实账单                                    │
   │  ┌──────────────────────────────────────────────────────┐   │
   │  │  SOUL.md (人格 / 语气 / stance)                       │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │  anti-patterns.md (honest limits)                    │   │
   │  ├──────────────────────────────────────────────────────┤   │
   │  │  evolution.jsonl (半自动迭代)                         │   │
   │  │   用户手动 append，下次加载作 reference                │   │
   │  └──────────────────────────────────────────────────────┘   │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 4: Validation (6 check)                                │
   │  A 组 4 check: stereotype / critique / refusal / trade-off   │
   │  +                                                            │
   │  B 组 2 check: anchor consistency / 族特定 check              │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 5: 交付                                                 │
   │  <sub-specialty-slug>/                                        │
   │    SOUL.md, SKILL.md, identity.json, evolution.jsonl,         │
   │    references/{capability-map, capability-clusters/,          │
   │                 anti-patterns, critique-rubric}.md            │
   │  +                                                            │
   │  GENERATION_REPORT.md (诚实账单 + anti-pattern audit)         │
   └──────────────────────────────────────────────────────────────┘
```

</details>

---

## 项目结构

本仓库本身就是一个可被 Claude Code 项目级加载的 dogfood 布局——skill 包放在 `.claude/skills/luban-skill/`，进入本仓库时 Claude Code 自动发现。要装到全局自用，复制该目录到 `~/.claude/skills/luban-skill/`。

```
./
├── README.md                         # 本文件
├── LICENSE                           # MIT
├── CHANGELOG.md                      # 版本历史
├── ARCHITECTURE_v0.2.md              # 架构决策稿（D1-D10 + 各批次产出 + v0.3.0 内化记录）
├── examples/                         # [v0.4] 计划：端到端示例
│   └── design-director-b2b-saas/
└── .claude/
    └── skills/
        └── luban-skill/              # skill 包根目录（项目级自动加载）
            ├── SKILL.md              # luban 元工具入口
            └── references/
                ├── generation-protocol.md    # 主生成流程（§0 7 立场 + §1-§15）
                ├── domain-families.md        # 5 个领域族骨架
                ├── evolution-protocol.md     # 半自动迭代流程
                ├── seed-prospect-protocol.md # 种子勘探报告格式
                ├── identity-schema.json      # identity.json 的 JSON Schema
                ├── soul-template.md          # SOUL.md 可选脚手架
                ├── skill-template.md         # 角色 SKILL.md 可选脚手架
                ├── capability-map-template.md
                ├── critique-rubric-template.md
                └── anti-patterns-template.md
```

所有 `*-template.md` 是**可选脚手架**，不是强制模板。

---

## Honest Limits（当前 v0.3.0）

- **种子质量决定上限**：LinkedIn 文章作种子和 design review 实录作种子，产出的角色差距巨大。鲁班无法补救坏种子。
- **快速演进领域（AI、加密、监管）需要持续更新种子**：v0.3.0 在 6 个月后可能就过时。
- **Validation 必要非充分**：6 个 check 全过不代表角色一定有用。最终判断标准是"真专业人士用了之后说有用"——这条鲁班自己测不了。
- **端到端示例尚未 ship**：v0.3.0 是元工具完成态，第一个真实蒸馏案例（Design Director B2B SaaS）在 v0.4。

---

## 立项动机

[colleague-skill](https://github.com/titanwings/colleague-skill) 证明了"蒸馏一个具体的人"可行。[Nuwa](https://github.com/alchaincyf/nuwa-skill) 把它推到极致——蒸馏 Munger、Naval、Musk 这类有海量公开语料的真人。

但大部分专业者面对的不是"我想要一个 Musk 在线对话"，而是"我想要一个能像资深 B2B SaaS PM 那样审视我 PRD 的判断者"。这不是蒸馏人，**是蒸馏专业方法论**。

蒸馏方法论的难点不在"如何让 LLM 扮演专家"——那是已知解决的问题，效果还很差（详见 generation-protocol §0 立场 1-2 对 vibes persona 的批判）。难点在两件事：

1. **专业是怎么形成的**：是 critique corpora（不是博客）、标准文档（不是描述）、失败案例（不是成功故事）的累积
2. **专业是怎么验证的**：是 6 个 check（A 组 stereotype / critique / refusal / trade-off + B 组 anchor consistency / 族特定 check）的全过，不是"我感觉它说得对"

`luban-skill` 把这两件事工程化为可执行的 protocol。

**鲁班造工具，但工具不是凭空想出来的——是工匠在 critique 失败、积累标准、记录失败案例中长出来的。luban-skill 就是这件事的元工具。**

---

## 当前进度 & Changelog

v0.3.0 已 ship——方法论内化重构完成，鲁班独立运行。完整版本历史见 [CHANGELOG.md](CHANGELOG.md)，架构决策稿见 [ARCHITECTURE_v0.2.md](ARCHITECTURE_v0.2.md)。

---

## 致谢与参考

luban 不是凭空长出来的。下面这些工作各自解决了"如何把一个角色/人格/专业能力工程化"的一部分,luban 站在它们的肩膀上,选了一条不同的路径(蒸馏 sub-specialty 的方法论,不是蒸馏个人 / 不做 persona portability)。

- **人格化思路 —— [DeepPersona: A Generative Engine for Scaling Deep Synthetic Personas](https://arxiv.org/abs/2511.07338)** (Wang et al., 2025)
  taxonomy-first + progressive specification 的两阶段框架。luban 的 Stage 1 "Capability Taxonomy Mining" → Stage 3 "Progressive Specification (5:3:2 sampling)" 与之同源,差异在 luban 拒绝 LLM 凭空生成 taxonomy,要求由 critique corpora 反推。

- **蒸馏启发 —— [Nuwa-skill](https://github.com/alchaincyf/nuwa-skill)** (alchaincyf)
  把"蒸馏一个具体的人"做到了工程级别,验证了 distillation-as-skill 可行。luban 是它的正交补集:Nuwa 蒸馏个人 mental model,luban 蒸馏 sub-specialty 的方法论。两个项目的分工在 [SKILL.md §6](.claude/skills/luban-skill/SKILL.md) 有明确边界。

- **SOUL 结构 —— [soul-protocol](https://github.com/qbtrix/soul-protocol)** (qbtrix)
  完整的 portable AI identity 规范(身份元数据 / OCEAN 人格 / 五层记忆 / 状态管理 / .soul 归档格式)。luban 的 `SOUL.md` 是它的极简子集——只保留"人格 / 语气 / stance"三件,不做 memory / portability / state,因为 luban 的关切是"专业判断",portability 不在主线。

- **SOUL 方法 —— [OpenClaw `SOUL.md` 概念](https://docs.openclaw.ai/concepts/soul)**
  "Where your agent's voice lives"——SOUL.md 作为人格层独立文件的定位由它确立。luban 直接采用此定位,并把它与 capability layer (SKILL.md) / identity layer (identity.json) 严格分开。

另外,立场 1-7 的概念骨架还借鉴了 Anthropic 内部研究方向 expert-persona-synthesis,详情见上方[方法论血统](#方法论血统)小节。

如果你的工作和 luban 相关、想被列入此处,欢迎开 issue。

---

## License

MIT
