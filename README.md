# 鲁班.skill (luban-skill)

> Nuwa 蒸馏人，鲁班蒸馏专业方法论。

[![Version: v0.3.0](https://img.shields.io/badge/version-v0.3.0-green)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)]()

**项目状态：v0.3.0 已 ship（方法论内化为自洽系统，可独立运行）。完整方法论文件包就绪，端到端示例 (v0.4) 在路上。**

---

## 一句话定位

`luban-skill` 是把"输入领域名 → 输出专业角色"这件事，**从'让 LLM 描述一个专家'升级为'按方法论蒸馏一个可被真专家承认的角色'**的 Claude Code skill。

区别于蒸馏真人个人风格（ [Nuwa.女娲](https://github.com/alchaincyf/nuwa-skill) ），鲁班生成的角色，更专注面向专业场景的交付，包括**该专业的判断方法、能力清单、决策启发法、自检标准**。

---

## 它和现有项目的关系

| 项目 | 核心命题 | 数据源 |
|---|---|---|
| [Nuwa](https://github.com/alchaincyf/nuwa-skill) | 蒸馏具体真人的 mental model | 该真人的书 / 演讲 / 推特（GB 级单人语料） |
| [OpenPersona](https://github.com/acnlabs/OpenPersona) | persona lifecycle（生成、约束、演化） | 用户自定义 |
| 各种 soul.md (clawsouls / rokoss21 / aaronjmars) | AI agent 人格 portability | SOUL.md 文件标准化 |
| **luban-skill** | **蒸馏专业方法论 + 强制 sub-specialty 锚定** | **该 sub-specialty 的 critique corpora（review 实录、标准文档、失败案例）** |

护城河在两件事：
1. **强制 sub-specialty**：不接受"产品经理"这种泛输入，必须收敛到"B2B SaaS PM"级别
2. **种子勘探模式**：用户没种子时，luban 主动产出"该 sub-specialty 的 critique corpora 候选清单"，让用户去找

---

## 方法论架构图

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

---

## 方法论：luban 的 7 条核心立场

完整定义在 [`luban-skill/references/generation-protocol.md` §0](luban-skill/references/generation-protocol.md)。摘要：

1. **拒绝 vibes persona**：禁止"You are a world-class designer with 20 years of experience"这类描述性 prompt——产出读起来像 LinkedIn 简介，没有真判断
2. **拒绝 LLM 凭空生成**：禁止让 LLM "describe an expert X" 来生成 capability——这是 stereotype 再生产，是大多数"AI 专家角色"项目的根本失败
3. **强制 sub-specialty**："senior designer" 太宽，必须拆到 "B2B SaaS UX designer" 级别
4. **数据源按信号密度排序**：critique corpora > 面试题库 > 标准文档 > 失败案例 > 实践者博客
5. **三层加载**（Tier-1 always loaded / Tier-2 per task family / Tier-3 retrieval on demand）：不是把所有能力塞进 SKILL.md，而是按使用频率分层
6. **5:3:2 progressive sampling**：在 Tier-1 内部选 5 条核心、3 条邻接、2 条远端能力——对抗 LLM 的刻板印象再生产
7. **6-check validation 非选项**：内容质量 4 + 结构一致性 2，全过才算交付

### 方法论血统

立场 1-7 的概念骨架来自 expert-persona-synthesis（Anthropic 内部研究方向）的吸收消化，加上 luban 在族骨架、SOUL 层、evolution 协议、种子勘探模式上的扩展。v0.3.0 起方法论已**内化为 luban 自身**——不再要求用户先安装 expert-persona-synthesis，luban 独立运行。

**最容易引起分歧的是立场 2（拒绝 LLM 凭空生成）和立场 7（validation 强度）**——这两条把"快速生成体验"和"严肃方法论"放在了对立面。luban 选择了后者。如果你的产品愿景是前者，luban 不适合你。

---

## 项目结构（v0.3.0）

仓库根目录（人类读文档 + 许可证）与 **Claude Code 可安装的 skill 目录** `luban-skill/` 分离：

```
./
├── README.md                         # 本文件
├── LICENSE                           # MIT
├── CHANGELOG.md                      # 版本历史
├── ARCHITECTURE_v0.2.md              # 架构决策稿（D1-D10 + 各批次产出 + v0.3.0 内化记录）
├── examples/                         # [v0.4] 计划：端到端示例，当前目录可不存在
│   └── design-director-b2b-saas/
└── luban-skill/                      # 安装到 Claude Code 的 skill 包根目录
    ├── SKILL.md                      # luban 元工具入口（YAML + 触发条件 + 加载顺序）
    └── references/
        ├── generation-protocol.md    # 主生成流程（§0 7 立场 + §1-§15）
        ├── domain-families.md        # 5 个领域族骨架
        ├── evolution-protocol.md     # 半自动迭代流程
        ├── seed-prospect-protocol.md # 种子勘探报告格式
        ├── identity-schema.json      # identity.json 的 JSON Schema
        ├── soul-template.md          # SOUL.md 可选脚手架
        ├── skill-template.md         # 角色 SKILL.md 可选脚手架（含 Anthropic 官方 YAML 规范）
        ├── capability-map-template.md
        ├── critique-rubric-template.md
        └── anti-patterns-template.md
```

**注**：所有 `*-template.md` 是**可选脚手架**，不是强制模板。生成时如果 sub-specialty 有特殊需求，可以完全自由组织。模板只是新用户的起点。

---

## 当前进度

**v0.3.0 已 ship**：方法论内化重构。luban 独立运行，不再外挂依赖 expert-persona-synthesis。

| 文件 | 状态 | 备注 |
|---|---|---|
| `luban-skill/SKILL.md` | ✅ v0.3 | YAML frontmatter 第三人称、≤1024 字符、符合 Anthropic 官方规范 |
| `README.md`（仓库根） | ✅ v0.3 | 含架构图、7 立场摘要、honest limits |
| `LICENSE` | ✅ v0.2 | MIT |
| `CHANGELOG.md` | ✅ v0.3 | v0.1.0 / v0.2.0 / v0.2.1 / v0.3.0 完整历史 |
| `ARCHITECTURE_v0.2.md` | ✅ v0.3 | D1-D10 + 各批次附录 + v0.3.0 内化记录 |
| `luban-skill/references/generation-protocol.md` | ✅ v0.3 | §0 重写为 luban 7 立场，§4-§13 内化无外部引用 |
| `luban-skill/references/domain-families.md` | ✅ v0.2 | 5 族骨架 |
| `luban-skill/references/identity-schema.json` | ✅ v0.3 | 删 depends_on 字段，描述去 EPS 引用 |
| `luban-skill/references/evolution-protocol.md` | ✅ v0.2 | 半自动迭代 + fold 机制 |
| `luban-skill/references/seed-prospect-protocol.md` | ✅ v0.2 | 零种子勘探规范 |
| `luban-skill/references/soul-template.md` | ✅ v0.2 | 可选脚手架 |
| `luban-skill/references/skill-template.md` | ✅ v0.3 | YAML 规范段按 Anthropic 官方重写 |
| `luban-skill/references/capability-map-template.md` | ✅ v0.3 | 去 EPS Stage X 引用 |
| `luban-skill/references/critique-rubric-template.md` | ✅ v0.3 | "EPS 通用层" → "通用层" |
| `luban-skill/references/anti-patterns-template.md` | ✅ v0.2 | 唯一嵌反例模板 |
| Design Director 示例 | ⏸ v0.4 | 端到端示例验证元框架 |


---

## Honest Limits（当前 v0.3.0）

- **种子质量决定上限**：LinkedIn 文章作种子和 design review 实录作种子，产出的角色差距巨大。luban 无法补救坏种子。
- **快速演进领域（AI、加密、监管）需要持续更新种子**：v0.3.0 在 6 个月后可能就过时。
- **Validation 必要非充分**：6 个 check 全过不代表角色一定有用。最终判断标准是"真专业人士用了之后说有用"，这条 luban 自己测不了。

---

## 立项动机（讲给社区听的版本）

[colleague-skill](https://github.com/titanwings/colleague-skill) 证明了"蒸馏一个具体的人"可行。[Nuwa](https://github.com/alchaincyf/nuwa-skill) 把它推到极致——蒸馏 Munger、Naval、Musk 这类有海量公开语料的真人。

但大部分专业者面对的不是"我想要一个 Musk 在线对话"，而是"我想要一个能像资深 B2B SaaS PM 那样审视我 PRD 的判断者"。这不是蒸馏人，**是蒸馏专业方法论**。

蒸馏方法论的难点不在"如何让 LLM 扮演专家"——那是已知解决的问题，效果还很差（详见 generation-protocol §0 立场 1-2 对 vibes persona 的批判）。难点在两件事：

1. **专业是怎么形成的**：是 critique corpora（不是博客）、标准文档（不是描述）、失败案例（不是成功故事）的累积
2. **专业是怎么验证的**：是 6 个 check（A 组的 stereotype / critique / refusal / trade-off + B 组的 anchor consistency / 族特定 check）的全过，不是"我感觉它说得对"

`luban-skill` 把这两件事工程化为可执行的 protocol。

**鲁班造工具，但工具不是凭空想出来的——是工匠在 critique 失败、积累标准、记录失败案例中长出来的。luban-skill 就是这件事的元工具。**

---

## License

MIT

---

## Authors

[TODO]
