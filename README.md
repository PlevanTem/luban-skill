<p align="right">
  🌐 <b>中文</b> · <a href="./README.en.md">English</a> · <a href="./README.ja.md">日本語</a>
</p>

# 鲁班.skill (luban-skill)

<p align="center">
  <img src="./intro.png" alt="luban-skill: 蒸馏专业方法论，而不是刻板印象 — distill expert methodology, not LinkedIn bios. 左侧原材料（critique reviews / postmortems / interview banks / standards docs / failure cases）经蒸馏器输出为结构化 SKILL 文档（anchors / critique rubric / honest limits）" width="780" />
</p>

> **Nuwa 蒸馏人，鲁班蒸馏专业方法论。**
> *Nuwa distills people. luban distills disciplines.*

**把 "AI 扮演专家" 升级为 "AI 真的懂这个专业"。**

- **是什么** · 把任何一门手艺（B2B SaaS PM / 刑事辩护律师 / UX 设计总监…）的方法论蒸馏成 Claude Code Skill，不是"扮演资深 X"的人设
- **给谁用** · 写 PRD / 做合规 / 排设计稿 / 做内容运营——想要真专家的批判，而不是 LinkedIn-bio 套话
- **怎么试** · 在 Claude Code 输入 `用鲁班蒸馏一个 B2B SaaS PM 角色` → 完整 skill 落到 `.claude/skills/<role>/`，自动注册 `/<role>` slash 命令

[![Version: v0.4.0](https://img.shields.io/badge/version-v0.4.0-green)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)]()
[![Skill: Claude Code](https://img.shields.io/badge/skill-Claude%20Code-orange)]()

---

## 效果对比

> 4 个真实场景，**subagent 实测调用本仓库已 ship 的 skill**（subagent 读完该 skill 的 SOUL / SKILL / critique-rubric / anti-patterns 后回应）。✅ 列是 skill 的真实输出，为表格紧凑做 editorial 压缩，**anchor 命名与结构性 move 保留原貌**。点 slash 命令直达 skill 目录。

| 场景 → Skill | ❌ "扮演资深 X" 通用 prompt | ✅ luban 蒸馏 skill 实测 |
|---|---|---|
| 「3 周上线 AI 销售邮件助手，用 LangChain，目标 50% 邮件 agent 自动处理」<br>→ [`/infra-pm`](.claude/skills/infra-pm/) · Mira | *"Great initiative! Add risk assessment, A/B test templates, align stakeholders."* | 你带来的是解决方案不是问题。「50% 自动处理」是 capability metric 不是 product metric——销售对草稿的 override 率才是，典型 **eval theater**。发邮件不可逆但 PRD 没 confirmation gate——**autonomy 通胀**。这阶段直接 LangChain 是 **framework shopping**。逆提案：3 周做 workflow（LLM 起草 / 销售一键发 / log diff），50 封真实邮件 inter-rater 当 golden，目标改成"销售平均修改量 <30%"，跑通再谈 autonomy。 |
| 「新游戏要 cyberpunk 风（霓虹 + 雨水反光），下周交 3 张关键 KV」<br>→ [`/game-art-director`](.claude/skills/game-art-director/) · Vera | *"Strong direction! Keep consistency, mind color theory, gather references."* | 停一下。"霓虹 + 雨水反光"是 **mood-board procrastination** 的开场——是 surface（Blade Runner 以来 40 年的 texture trick）不是 north-star。在已有 cyberpunk 光谱（2077 工业巴洛克 / Edgerunners 高饱手绘 / Cloudpunk voxel）里，你"删掉就不成立"的 3 个 keyword 是什么——"corporate sterile" 还是 "street-level grime"，"neon as warmth" 还是 "neon as threat"？答不出就别开始 KV。这失败叫 **Homogenization to trend**。 |
| 「100 人问卷验证新功能值不值得开发」<br>→ [`/ux-research-director`](.claude/skills/ux-research-director/) · Wren | *"Great approach! Representative sample, pre-test, avoid leading questions."* | 先停一下——method 和 decision 是错配的。"值不值得做"要 behavioral evidence（用户付了什么代价、绕了什么 workaround），survey 给的是 stated preference——经典 **stated-vs-revealed 错配**。N=100 无 segmentation 是 **deceptively-easy survey** 陷阱。更深：finding 是"用户不要" PM 真会砍吗？不会就是 **validate-the-design**。先把 decision owner / 延后成本 / "反向 finding 会不会被接受" 写来，再谈方法。 |
| 「B2B SaaS 每周发 5 篇 LinkedIn 但 pipeline 没动——cadence 不够还是质量问题？」<br>→ [`/content-ops-director`](.claude/skills/content-ops-director/) · Lin | *"Great cadence! Post consistently, engage with comments, A/B test hooks."* | 先把这个二选一拒掉——都是 craft 层症状，根因 99% 在 system 层。停在 **documented strategy gate**：ICP 是谁、buyer journey 哪段在 LinkedIn 决策、3-5 个 content pillar、季度 theme——答不全 5 篇/周就是 **frequency-driven calendar + vanity-metrics 决策**。再问 2 个 system 信号：brand page 还是 employee advocacy？（employee reach 是 brand page 8x）5 篇是 1 个 monthly core asset 的 fanout 还是 5 个独立选题？后者是 **over-engineered frequency table**，不是 buyer-journey × pillar 矩阵。 |

**差距不来自"更好的 prompt"**——每个 skill 的 SOUL.md 默认拒绝套话，critique-rubric 强制结构化检查，anti-patterns 把 *"eval theater" / "autonomy 通胀" / "Homogenization to trend" / "validate-the-design" / "frequency-driven calendar" / "deceptively-easy survey"* 这些写成了**具名失败模式**——它们不是 prompt 临时想出来的，是 skill 定义里的固定 anchor。**立场是结构性的**——详见 [工作原理](#工作原理)。

---

## 鲁班能为你做什么

已 ship 的 4 个 skill 直接覆盖这些场景：

- **写 PRD / 评 AI agent 基建** → [`/infra-pm`](.claude/skills/infra-pm/) · Mira the PM，0→1 PMF 阶段真懂 Anthropic Building Effective Agents 立场，挑刺不套话
- **做内容运营 / 排海内外社媒** → [`/content-ops-director`](.claude/skills/content-ops-director/) · Lin，B2B SaaS 跨地区，measurement-first + employee 8x leverage
- **做 UX 调研决策** → [`/ux-research-director`](.claude/skills/ux-research-director/) · Wren，Erika Hall 路线先质疑 method-decision fit，不是优化问卷
- **定游戏视觉方向** → [`/game-art-director`](.claude/skills/game-art-director/) · Vera，关键词驱动 visual DNA，拒绝 mood-board procrastination

**自己没有现成的 skill？** 用 luban 蒸馏一个（律师 / 财务顾问 / 投资人 / design director / 合规 / 并购…），鲁班不取代该专业的人，**它把"判断该专业活儿做得好不好的标准"工程化成可执行的 Skill**。

---

## 快速开始

<p align="center">
  <img src="./usage.svg" alt="鲁班使用说明 3 步：① 你投料（critique reviews / postmortems / standards docs / interview banks / failure cases，或零种子进入勘探模式）→ ② 鲁班蒸馏（5 stage pipeline：taxonomy mining / anchor / 5:3:2 progressive spec / critique rubric / tools &amp; workflow，落到 .claude/skills/&lt;role&gt;/）→ ③ 你召唤 /&lt;role&gt;，在 Claude Code 里得到会挑刺、不是套话的真专业评审" width="1100" />
</p>

**直接试一下**——进入本仓库时 Claude Code 会自动加载 luban-skill。在 Claude Code 对话框输入：

```
用鲁班蒸馏一个 B2B SaaS PM 角色，我已经有 30 份 design review 实录
```

或者**没有种子材料**：

```
用鲁班帮我蒸馏一个刑事辩护律师，我手头什么都没有
```

luban 会进入**种子勘探模式**，给你一份"该 sub-specialty 的 critique corpora 候选清单"——告诉你去找什么、在哪找、按什么顺序找。

**产物**：一份完整角色目录落到 `.claude/skills/<short-slug>/`，含 `SOUL.md` + `SKILL.md` + `identity.json` + capability map + critique rubric + anti-patterns + 诚实账单 `GENERATION_REPORT.md`。Claude Code 自动发现，`/<short-slug>` 在浮动面板可见。

**浏览全部蒸馏角色**：输入 `/agents`，结构化 roster 按 family 分组渲染。

<details>
<summary>装到全局自用 / 在其他项目调用</summary>

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

装完后任何项目里都能召唤 luban 蒸馏新 skill。

</details>

---

## Why luban (not another persona prompt)

市面上 90% "AI 扮演专家" 项目栽在两件事上：

1. **Vibes persona**：`"You are a senior X with 20 years of experience"` 这类描述性 prompt 产出 LinkedIn 简介式 voice，看起来对、抓不到真问题。
2. **LLM 凭空生成 capability**：让 LLM 自己 "describe what a senior X knows" —— 这是 stereotype 再生产，是大多数 persona repo 的根本失败。

鲁班拒绝这两条。**Capability 必须从 critique corpora / 标准文档 / 失败案例反推**，不是从 LLM 想象拉出。这是结构性立场，不是 prompt 调音能补的。

---

## 已蒸馏的 Skill 示例

| Sub-specialty | Slash | Display name | 状态 |
|---|---|---|---|
| Agent infrastructure PM (0→1 PMF) | [`/infra-pm`](.claude/skills/infra-pm/) | Mira the PM | ✅ v0.3.0 ship |
| Game Art Director / Visual Lead (0→1 视觉定调) | [`/game-art-director`](.claude/skills/game-art-director/) | Vera | ✅ v0.1.0 ship |
| Generalist UX Research Director | [`/ux-research-director`](.claude/skills/ux-research-director/) | Wren | ✅ v0.1.0 ship |
| B2B SaaS Content Ops Director (cross-region) | [`/content-ops-director`](.claude/skills/content-ops-director/) | Lin | ✅ v0.1.0 ship |

> v0.4.0 把元工具方法论真的跑出了 4 个角色——全部用 luban 自己蒸馏。欢迎 PR 贡献新的 sub-specialty。

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

<details>
<summary>展开：5 阶段蒸馏流水线 + 完整架构图</summary>

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
   │  Stage 1: Capability Taxonomy Mining → capability-map.md     │
   │  Stage 2: Anchor → identity.json                             │
   │  Stage 3: Progressive Specification (5:3:2) → SKILL.md       │
   │  Stage 4: Critique Rubric → critique-rubric.md               │
   │  Stage 5: Tools & Workflow → SKILL.md workflow section       │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 3: 人格层 + 诚实账单                                    │
   │  SOUL.md + anti-patterns.md + evolution.jsonl                │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 4: Validation (6 check)                                │
   │  A 组 4 check + B 组 2 check                                  │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 5: 交付                                                 │
   │  <sub-specialty-slug>/ + GENERATION_REPORT.md                │
   └──────────────────────────────────────────────────────────────┘
```

</details>

---

## 项目结构

本仓库就是一个可被 Claude Code 项目级加载的 dogfood 布局——所有 skill 都在 `.claude/skills/` 下，Claude Code 自动发现。

```
./
├── README.md / README.en.md / README.ja.md   # 多语言入口（中文默认）
├── intro.png / usage.svg(.en/.ja)            # banner + 快速开始插图
├── LICENSE / CHANGELOG.md / ARCHITECTURE_v0.2.md
└── .claude/skills/
    ├── INDEX.md                              # 蒸馏角色注册表
    ├── luban-skill/                          # 元工具（生成新角色）
    ├── agents/                               # /agents meta-skill
    ├── infra-pm/                             # Mira the PM
    ├── game-art-director/                    # Vera
    ├── ux-research-director/                 # Wren
    └── content-ops-director/                 # Lin
```

每个角色目录含 `SOUL.md` + `SKILL.md` + `identity.json` + `references/` (capability-map / critique-rubric / anti-patterns / source-material...)。

---

## 当前进度 & Changelog

v0.3.0 已 ship——方法论内化重构完成，鲁班独立运行。v0.4.0 已蒸馏 4 个角色。完整版本历史见 [CHANGELOG.md](CHANGELOG.md)。

---

<details>
<summary><b>立项动机</b>（为什么做这件事 / 和 Nuwa 等项目的关系）</summary>

[colleague-skill](https://github.com/titanwings/colleague-skill) 证明了"蒸馏一个具体的人"可行。[Nuwa](https://github.com/alchaincyf/nuwa-skill) 把它推到极致——蒸馏 Munger、Naval、Musk 这类有海量公开语料的真人。

但大部分专业者面对的不是"我想要一个 Musk 在线对话"，而是"我想要一个能像资深 B2B SaaS PM 那样审视我 PRD 的判断者"。这不是蒸馏人，**是蒸馏专业方法论**。

蒸馏方法论的难点不在"如何让 LLM 扮演专家"——那是已知解决的问题，效果还很差。难点在两件事：**专业是怎么形成的**（critique corpora、标准文档、失败案例的累积，而非博客 / 描述 / 成功故事）+ **专业是怎么验证的**（6 个 check 全过，而非"我感觉它说得对"）。

`luban-skill` 把这两件事工程化成可执行的 protocol。**工具不是凭空想出来的——是工匠在 critique 失败、积累标准、记录失败案例中长出来的。luban-skill 就是这件事的元工具。**

</details>

<details>
<summary><b>致谢与参考</b></summary>

luban 不是凭空长出来的。下面这些工作各自解决了"如何把一个角色 / 人格 / 专业能力工程化"的一部分，luban 站在它们的肩膀上，选了一条不同的路径（蒸馏 sub-specialty 的方法论，不是蒸馏个人 / 不做 persona portability）。

- **人格化思路 —— [DeepPersona: A Generative Engine for Scaling Deep Synthetic Personas](https://arxiv.org/abs/2511.07338)** (Wang et al., 2025) — taxonomy-first + progressive specification 的两阶段框架。luban 同源，差异在拒绝 LLM 凭空生成 taxonomy。
- **蒸馏启发 —— [Nuwa-skill](https://github.com/alchaincyf/nuwa-skill)** (alchaincyf) — 把"蒸馏一个具体的人"做到工程级别。luban 是它的正交补集：Nuwa 蒸馏个人 mental model，luban 蒸馏 sub-specialty 方法论。
- **SOUL 结构 —— [soul-protocol](https://github.com/qbtrix/soul-protocol)** (qbtrix) — 完整的 portable AI identity 规范。luban 的 `SOUL.md` 只取"人格 / 语气 / stance"极简子集，不做 portability。
- **SOUL 方法 —— [OpenClaw `SOUL.md` 概念](https://docs.openclaw.ai/concepts/soul)** — "Where your agent's voice lives" 确立了 SOUL.md 作为人格层独立文件的定位。

如果你的工作和 luban 相关、想被列入此处，欢迎开 issue。

</details>

---

## License

MIT
