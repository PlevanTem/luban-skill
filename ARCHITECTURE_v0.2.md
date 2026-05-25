# luban-skill v0.2 架构确认稿

> 这是一份**决策文档**，不是项目文件。
> 它的目的是把所有"已经写在 README 里但没经过你和我共同确认"的判断显式化，让你拍板后再写实际文件。
> 审完这份后会进入分批写文件阶段。

---

## 0. 当前事实清单

> **路径勘误（相对本仓库）**：方法论与模板文件在 `luban-skill/references/`；`README.md` 等在仓库根。下文若仍写 `references/...` 而未加前缀，在审阅本稿时可一律理解为 `luban-skill/references/...`。

**已存在的文件**（825 行）：
- `README.md`（仓库根）— 229 行 — 来源不明，我没在对话里创建它，但内容方向和 v2 一致
- `luban-skill/references/generation-protocol.md` — 332 行 — 我 v2 重写
- `luban-skill/references/domain-families.md` — 264 行 — 我 v2 重写

**README 里"项目结构（计划）"声称要做的文件**（13 个，含已有的）：
```
luban-skill/
├── SKILL.md                          # [TODO]
├── README.md                         # ✅（注：当前仓库中 README 实际在根目录，与 skill 包分离）
├── references/
│   ├── generation-protocol.md        # ✅
│   ├── domain-families.md            # ✅
│   ├── soul-template.md              # [TODO]
│   ├── skill-template.md             # [TODO]
│   ├── identity-schema.json          # [TODO]
│   ├── capability-map-template.md    # [TODO]
│   ├── critique-rubric-template.md   # [TODO]
│   ├── anti-patterns-template.md     # [TODO]
│   ├── evolution-protocol.md         # [TODO]
│   └── seed-prospect-template.md     # [TODO]
├── examples/
│   └── design-director-b2b-saas/     # [TODO]
└── LICENSE                           # [TODO]
```

待补：10 个文件 + 1 个示例目录。本轮不做示例，按你的指令。

---

## 1. 架构层级图（必须先对齐）

luban-skill 的文件分**三层**，理解错层级会导致内容串味。

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: SKILL 入口层                                       │
│  Claude Code 加载的入口文件，触发器声明                       │
│  ├─ SKILL.md                                                │
│  └─ README.md (人类读，介绍项目)                              │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: 方法论文档层 (references/)                          │
│  解释"luban 怎么生成角色"的规则文档                            │
│  ├─ generation-protocol.md (主流程)                          │
│  ├─ domain-families.md (族骨架)                              │
│  ├─ evolution-protocol.md (迭代流程)                          │
│  └─ seed-prospect-template.md (种子勘探报告格式)              │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: 模板层 (references/*-template.md)                  │
│  生成的角色目录里每个文件长什么样的填空模板                     │
│  ├─ soul-template.md                                        │
│  ├─ skill-template.md                                       │
│  ├─ capability-map-template.md                              │
│  ├─ critique-rubric-template.md                             │
│  ├─ anti-patterns-template.md                               │
│  └─ identity-schema.json                                    │
└─────────────────────────────────────────────────────────────┘
```

**关键区分**：
- **方法论文档**告诉 Claude（执行者）**怎么做** —— 是"读了之后知道走哪一步"
- **模板**告诉 Claude **生成的文件长什么样** —— 是"读了之后知道每个字段填什么"

`evolution-protocol.md` 属于方法论文档（说"如何迭代"）；
`*-template.md` 都是模板（说"文件长什么样"）。

`seed-prospect-template.md` 命名上是 template，但本质是方法论——它描述"种子勘探报告"这个产物的格式。归到 Layer 2 还是 Layer 3 是个判断（见 §3 决策 D5）。

---

## 2. 每个文件的定位

### Layer 1: 入口层

**SKILL.md**
- 用途：Claude Code 加载本 skill 时读的入口，声明 trigger 关键词、加载顺序、安全边界
- 字数预估：100-200 行
- 依赖：所有 references 文件（用 `view` 引用，不内联）
- 关键决策：trigger 用什么关键词触发？参考 §3 决策 D1
- 真实约束：必须有 YAML frontmatter（`name:` 和 `description:`）才能被 Claude Code 识别为 skill

**README.md**（已存在）
- 用途：项目展示，给社区看
- 当前问题：229 行里有几处未经确认的判断（"加强 2 check"、"东亚/欧美 corpora 失衡"等）
- 处理：保留主体，但需要小修。见 §3 决策 D2

### Layer 2: 方法论文档

**generation-protocol.md**（已存在）
- 332 行 v2，结构稳定。本轮**不重写**，但可能在 §4 任务时小修

**domain-families.md**（已存在）
- 264 行 v2，结构稳定。本轮**不重写**

**evolution-protocol.md**（新建）
- 用途：描述用户如何半自动迭代生成的角色
- 内容：evolution.jsonl 格式 / 何时 append / 用户审核流程 / 不能做的事（自动演化等）
- 字数预估：80-150 行
- 依赖：generation-protocol.md §11

**seed-prospect-template.md**（新建，命名见 §3 D5）
- 用途：零种子模式下，luban 主动产出的"corpora 候选清单"长什么样
- 字数预估：60-120 行
- 依赖：generation-protocol.md §3、domain-families.md 的 corpora 候选类型

### Layer 3: 模板

**identity-schema.json**（新建）
- 用途：identity.json 的 JSON Schema 规范
- 内容：generation-protocol.md §10 里的 schema 的正式化版本，加上字段说明
- 字数预估：80-120 行
- 依赖：generation-protocol.md §10

**soul-template.md**（新建）
- 用途：SOUL.md 文件的模板与填写指南
- 内容：5 个 section（Tone / Stance / Brevity rule / No-go phrases / When to push back）的格式 + 反例 + OpenClaw 的克制原则摘要
- 字数预估：100-180 行
- 依赖：generation-protocol.md §9

**skill-template.md**（新建）
- 用途：角色的 SKILL.md 长什么样
- 内容：YAML frontmatter + When to use / When NOT to use / Core capabilities / Workflow / References
- 字数预估：120-180 行
- 依赖：generation-protocol.md §6（Stage 3 Tier-1 内容）+ §8（Stage 5 workflow）
- 关键决策：是否要求生成的角色都长得一样格式？见 §3 决策 D3

**capability-map-template.md**（新建）
- 用途：capability-map.md 长什么样
- 内容：3 层 markdown 树的格式 / 叶节点必须 actionable 的检查规则 / `[unverified]` 标记规则
- 字数预估：80-150 行
- 依赖：generation-protocol.md §4（Stage 1）+ expert-persona-synthesis Stage 1

**critique-rubric-template.md**（新建）
- 用途：critique-rubric.md 长什么样
- 内容：Before answering / After drafting 两段的格式 + 通用层 + 族特定层 + 不允许的空话清单
- 字数预估：80-130 行
- 依赖：generation-protocol.md §7（Stage 4）+ domain-families.md 各族 critique 检查项

**anti-patterns-template.md**（新建）
- 用途：anti-patterns.md 长什么样
- 内容：三类内容（做不到的事 / 容易犯的错 / 应让位的情况）+ zero-shot 模式的强制条款
- 字数预估：80-130 行
- 依赖：generation-protocol.md §12 + domain-families.md 各族 anti-pattern

### Layer 0: 治理

**LICENSE**（新建）
- 用途：开源协议
- 内容：MIT（README 已声明）
- 字数预估：21 行（标准 MIT 文本）

---

## 3. 未决判断清单（必须你拍板）

以下是 README / v2 reference / 项目结构里**已经被假设、但没经过你和我明确确认**的判断。每条标:

- **D 编号**
- **是什么判断**
- **现状**（README/v2 里是怎么写的）
- **我的推荐**
- **备选**
- **影响范围**

---

### D1: SKILL.md 的 trigger 关键词

**判断**：用户说什么样的话，Claude Code 会加载 luban-skill？

**现状**：README 里说"输入领域名自动生成"，但没明确触发词

**我的推荐**：
trigger 关键词包括：
- "生成一个 X 的专家角色"、"create an expert persona for X"
- "用鲁班生成"、"use luban"
- "蒸馏 X 这个专业"、"distill X profession"
- 提到"专业角色"、"expert agent"、"professional persona"

**备选**：
- A. 上述全套
- B. 只用显式 trigger（必须说"luban"/"鲁班"才触发）—— 避免误触发
- C. 隐式 trigger 更宽（任何"扮演 X 专家"的请求都触发）—— 用户认知成本低但容易和 expert-persona-synthesis 撞触发

**影响**：决定 SKILL.md 的 description 字段怎么写

---

### D2: README 里残留的未审核判断怎么处理

**判断**：README 里这几条单方面加的内容是否保留？

**残留清单**：
1. "luban 加强 2 check: anchor consistency / 族特定 check"——v2 generation-protocol 里没有
2. Honest Limits 里的"东亚/欧美 critique corpora 不平衡"——v2 domain-families 没讨论
3. "鲁班造工具但工具不是凭空想出来的——是工匠在 critique 失败、积累标准、记录失败案例中长出来的"——立项动机段落的金句
4. README 项目结构里的 `decision-heuristics.md` —— v2 generation-protocol 没列这个文件

**我的推荐**：
- 第 1 条："加强 2 check"是好想法但 generation-protocol 没实现 → 要么补到 generation-protocol（推荐），要么从 README 删掉。**我推荐补到 generation-protocol §13 作为 luban 增量**
- 第 2 条：是有判断力的观察 → 保留，但移到一个新文件 `references/known-biases.md`（或合并到 generation-protocol §15 honest limits）
- 第 3 条：保留，是好叙事
- 第 4 条：`decision-heuristics.md` 是 v1 的残留——v2 里 heuristics 内容已经被 `capability-map.md` + `critique-rubric.md` 覆盖。**我推荐从 README 项目结构里删掉**，文件不做

**备选**：
- A. 按推荐处理（4 条各有处置）
- B. 全部从 README 删掉，回到 v2 状态
- C. 全部保留，generation-protocol 反过来跟上 README

---

### D3: 生成的角色 SKILL.md 是否要求统一格式

**判断**：luban 生成的 100 个角色，SKILL.md 是否长得一样（统一模板），还是允许族特定差异？

**现状**：v2 没明确

**我的推荐**：
- **统一骨架**：YAML frontmatter / When to use / When NOT to use / Core capabilities / Workflow / References —— 这些 section 顺序和名字不变
- **族特定填充**：每个 section 内部的内容由族决定。例如 Family 1 (Legal) 的 Workflow 一定包含"管辖权确认"步骤，Family 2 (Engineering) 一定包含"约束条件评估"步骤

**备选**：
- A. 统一骨架 + 族特定填充（我推荐）
- B. 完全统一（所有角色 SKILL.md 一模一样）—— 简单但丢失族差异
- C. 完全自由（每个角色 SKILL.md 自己写）—— 灵活但下游工具无法统一处理

**影响**：决定 skill-template.md 的写法

---

### D4: capability-map.md 三层 markdown 树的具体格式

**判断**：3 层是用嵌套 markdown 列表（- - -），还是用 H2/H3/H4 标题？

**现状**：generation-protocol §4 只说"3 层 markdown 树"，没指定语法

**我的推荐**：
H2 = 一级分支（6-12 个），H3 = 二级，无序列表叶节点。理由：
- 可以用文档大纲工具直接看树
- 叶节点用列表能自然加 `[unverified]` 标记
- Claude 在 view 时不会因层级太深而看不清

**备选**：
- A. H2/H3/列表 叶节点（我推荐）
- B. 全嵌套列表（- - -）—— 紧凑但层级深时难读
- C. JSON / YAML 树 —— 机器可读但人难写

**影响**：决定 capability-map-template.md 的写法

---

### D5: seed-prospect 文件归层 & 命名

**判断**：`seed-prospect-template.md` 在 Layer 2（方法论）还是 Layer 3（模板）？

**问题**：它的命名带 -template，但内容是描述"种子勘探报告"这种产物——属于方法论而不是模板

**我的推荐**：
- 归 Layer 2，**重命名为** `seed-prospect-protocol.md`
- 一致性：和 `evolution-protocol.md` 对称
- 删掉 README 项目结构里的 `seed-prospect-template.md`，改成 `seed-prospect-protocol.md`

**备选**：
- A. 重命名为 `-protocol`（我推荐）
- B. 保留 `-template` 命名，归 Layer 3
- C. 拆成两个：`seed-prospect-protocol.md`（方法论）+ `seed-prospect-template.md`（实际报告模板）—— 更纯但啰嗦

---

### D6: identity-schema.json 是规范还是示例

**判断**：这个文件里写的是 JSON Schema（机器可校验），还是带注释的 JSON 示例（人可读）？

**我的推荐**：
**JSON Schema**（`$schema: draft-07`），加上每个字段的 description。理由：
- 可以被工具校验
- description 字段能塞清晰说明
- 标准格式，未来扩展不混乱

**备选**：
- A. JSON Schema（我推荐）
- B. 带注释的 JSON 示例（更易读但不可校验）
- C. 两个都做（schema + example）—— 但增加维护成本

---

### D7: 模板里要不要内嵌"反例"

**判断**：soul-template.md / skill-template.md / 等等，要不要在模板里嵌入"❌ 错误写法 vs ✅ 正确写法"对比？

**我的推荐**：
**要**。expert-persona-synthesis 自己就用了大量反例（"避免 LinkedIn 简介式描述"、"不要用'年限'做 seniority proxy"）。模板没有反例 = Claude 生成时容易回滚到 vibes 模式。

**备选**：
- A. 每个模板都嵌反例（我推荐）—— 模板字数增加 20-30%
- B. 只在关键模板（SOUL / SKILL）嵌反例
- C. 不嵌，模板纯净，反例放进 generation-protocol

---

### D8: 是否在本轮生成 `references/known-biases.md`

**判断**：D2 提到把"东亚/欧美 corpora 失衡"放进新文件，要不要这一轮做？

**现状**：README 项目结构没列这个文件

**我的推荐**：
**做**。这是 luban 的诚实账单的一部分，应该独立成文便于维护。10-20 行短文档。

**备选**：
- A. 做（我推荐）—— 多 1 个文件，总数 14
- B. 不做，合并到 generation-protocol §15
- C. 不做，等真正发现更多偏差再说

---

### D9: LICENSE 选择

**判断**：MIT vs Apache 2.0 vs MIT + commercial restriction

**现状**：README 写了 MIT

**我的推荐**：
**MIT**。理由：
- Nuwa 用 MIT，对标项目一致
- Anthropic skills.sh 生态多用 MIT
- 不引入专利条款负担（Apache 2.0 的额外条款对个人项目无意义）

**备选**：
- A. MIT（我推荐）
- B. Apache 2.0
- C. CC BY-SA 4.0（强迫衍生作品开源）

---

### D10: 是否在 v0.2 里加 CHANGELOG.md

**判断**：项目有版本迭代历史，需不需要 CHANGELOG？

**我的推荐**：
**不做 v0.2 单独的 CHANGELOG**。在 README 末尾加一个"Changelog"小节就够了。理由：
- 项目还在 v0.x 早期，每 release 一份完整 CHANGELOG 是过早工程化
- 独立 CHANGELOG 是 v1.0+ 才需要的

**备选**：
- A. README 末尾小节（我推荐）
- B. 独立 CHANGELOG.md
- C. 不记录历史

---

## 4. 最终文件清单（待你拍板后定稿）

如果所有 D1-D10 都按我推荐走，最终文件清单是：

```
luban-skill/
├── SKILL.md                              [新建]
├── README.md                             [小修，按 D2 推荐]
├── LICENSE                               [新建, MIT]
├── references/
│   ├── generation-protocol.md            [可能小修，按 D2 第 1 条加 anchor consistency check]
│   ├── domain-families.md                [不变]
│   ├── evolution-protocol.md             [新建]
│   ├── seed-prospect-protocol.md         [新建, D5 重命名]
│   ├── known-biases.md                   [新建, D8]
│   ├── identity-schema.json              [新建, JSON Schema, D6]
│   ├── soul-template.md                  [新建, 含反例 D7]
│   ├── skill-template.md                 [新建, 统一骨架 D3, 含反例 D7]
│   ├── capability-map-template.md        [新建, H2/H3/列表 D4, 含反例 D7]
│   ├── critique-rubric-template.md       [新建, 含反例 D7]
│   └── anti-patterns-template.md         [新建, 含反例 D7]
└── examples/                             [本轮不做, 留空目录或不建]
```

**总计 14 个文件**（含本轮不做的 examples 占位）。
本轮要做：**10 个新建 + 2 个小修** = 12 个文件编辑动作。
不做：Design Director 示例。

---

## 5. 分批顺序（拍板后执行）

按依赖链：

### 批 1（独立、不互相依赖）
- LICENSE (MIT 标准文本)
- identity-schema.json (JSON Schema)
- known-biases.md (短文档)
- generation-protocol.md 小修 (按 D2 加 anchor consistency check)
- README.md 小修 (按 D2 处理残留)

约 5 个文件编辑动作。

### 批 2（互相依赖，必须一起做）
- soul-template.md
- skill-template.md
- capability-map-template.md
- critique-rubric-template.md
- anti-patterns-template.md

5 个模板互相引用，一致性必须保证。一批做完。

### 批 3（方法论文档，依赖批 2 的模板格式）
- evolution-protocol.md
- seed-prospect-protocol.md
- SKILL.md (入口，引用所有模板)

3 个文件。

### 总计 3 批，13 个文件编辑动作

每批结束我会停下让你审视，不一次性输出。

---

## 6. 我有 70% 信心的判断 vs 你需要看清的事

### 我有信心的判断（不需要你太细看）
- 层级图（§1）—— 文件分层逻辑
- 文件清单（§4）—— 14 个文件的归属
- 分批顺序（§5）—— 依赖链合理

### 必须你拍板的（不拍板我不能继续）
- D1: trigger 关键词
- D3: 生成角色的 SKILL.md 是否统一格式
- D7: 模板里嵌反例还是不嵌
- D9: LICENSE 选择

### 我推荐但你可能不同意
- D2: README 残留 4 条的处置
- D5: seed-prospect 重命名
- D8: 加 known-biases.md

### 工程细节（你可以授权我自己决定）
- D4: capability-map 用 H2/H3/列表
- D6: identity-schema.json 用 JSON Schema
- D10: 不做独立 CHANGELOG

---

## 7. 这份确认稿的 honest limits

- **没有验证过 SKILL.md 在 Claude Code 里能否被识别**——我假设它需要 YAML frontmatter，但没测试过。第一批写完应该手动验证一次
- **模板互相依赖关系是我推理出来的**，不是实际跑过的——批 2 写完时可能发现 §1 层级图需要调整
- **D7 嵌反例**——会让模板变长。如果你目标是极简模板，应该选 D7 备选 C
- **示例（examples/）的缺失会让 v0.2 看起来"齐全但没跑过"**——这是有意识的取舍，你已经同意"先方法论后示例"

---

## 等待你的输入

- **D1 / D2 / D3 / D5 / D7 / D8 / D9** 这 7 个判断需要你确认 / 修改 / 给备选
- D4 / D6 / D10 你可以授权我自己决定（如果你没意见就按我推荐）
- 整个文件清单（§4）有任何要增删的，现在说

确认完我开始批 1。

---

## 附录 A: 用户最终决策记录 (2026-05-25)

下表是用户拍板的最终决策。**不修改本稿原文**——本稿是"决策稿"，原文保留决策过程，附录记录最终结果。两者并存，避免伪造决策路径。

| D | 用户选择 | 备注 |
|---|---|---|
| D1 | **B** | 显式 trigger only ("luban" / "鲁班") |
| D2 | **A (部分)** | 加强 2 check 补到 generation-protocol；金句保留；decision-heuristics.md 删；**东亚/欧美 corpora 偏差不写入交付物** (用户特别声明) |
| D3 | **C** | 完全自由——每个角色 SKILL.md 自己写，不强制统一格式 |
| D4 | **A** | H2/H3 + 无序列表叶节点 |
| D5 | **A** | seed-prospect-protocol.md |
| D6 | **A** | JSON Schema draft-07 |
| D7 | **修订**：仅 anti-patterns-template.md 嵌反例，其他 4 个模板都不嵌 | 与原 B 选项相反 |
| D8 | **C** | 不做 known-biases.md |
| D9 | **A** | MIT |
| D10 | **B** | 独立 CHANGELOG.md |

矛盾解决：

- **矛盾 1** (D2+D8 冲突)：用户裁定——"东亚/欧美偏差不应在交付物里衡量"。从 README honest limits 删除，不另开 known-biases.md
- **矛盾 2** (D3 完全自由后 -template.md 何去何从)：保留 `*-template.md` 命名，内容定位为"**可选脚手架**"——每个模板第一段统一声明此事

附加约定 (用户拒绝独立 template-conventions.md 后)：

- 一致性由 Claude 在批 2 过程中自己保证，不写成文档
- Claude 内部基准（不写入项目文件）：统一首段声明语 + 统一引用格式 (`详见 generation-protocol §N`) + 统一占位符 `<填充：...>` + soul/skill 模板嵌"指针"指向 anti-patterns 但不嵌反例本身

---

## 附录 B: 批 1 实际产出 (2026-05-25)

按附录 A 决策，批 1 产出：

- ✅ `LICENSE` (MIT, 21 行)
- ✅ `CHANGELOG.md` (78 行)
- ✅ `references/identity-schema.json` (JSON Schema draft-07, 138 行)
- ✅ `references/generation-protocol.md` 改动：§13 加 luban 2 check，§14 改 "4 check" 为 "6 check"
- ✅ `README.md` 改动：删 decision-heuristics、删东亚/欧美偏差 honest limit、立项动机改 6 check、版本 badge 改 v0.2.0、当前进度表更新

未做 (Claude 一致性失误，需在批 2 修)：
- (无)

批 2 范围：5 个 `*-template.md` 模板，按附录 A 决策一并产出。

---

## 附录 C: 批 2 实际产出 (2026-05-25)

按附录 A 决策 + Claude 内部一致性基准，批 2 产出 5 个模板：

- ✅ `references/soul-template.md` (113 行) — 5 section + 嵌指针不嵌反例
- ✅ `references/skill-template.md` (143 行) — 角色 SKILL.md 脚手架 + Tier-1 选择标准 + 嵌指针不嵌反例
- ✅ `references/capability-map-template.md` (142 行) — H2/H3/列表 (D4) + actionability 测试 + 不嵌反例
- ✅ `references/critique-rubric-template.md` (130 行) — 三层叠加 + Sacred check + 不嵌反例
- ✅ `references/anti-patterns-template.md` (151 行) — **唯一嵌反例**模板 (D7 修订后唯一例外)

D7 范围最终确认：5 个模板中 **唯有 anti-patterns 嵌反例**，soul/skill 仅在开头加"嵌指针"（一句话引用 anti-patterns）。

批 3 范围：evolution-protocol + seed-prospect-protocol + 项目根 SKILL.md。

---

## 附录 D: 批 3 实际产出 (2026-05-25)

按附录 A 决策，批 3 产出最后 3 个文件：

- ✅ `references/evolution-protocol.md` — 半自动迭代流程，含 entry schema、触发场景、何时不应写入、fold 操作
- ✅ `references/seed-prospect-protocol.md` — corpora-candidates.md 格式规范，资源具体性要求，信号密度 vs 可访问性取舍
- ✅ `SKILL.md` — luban 项目根入口，Claude Code 加载触发器（D1 显式 trigger only），加载顺序，关键决策点

v0.2.0 文件清单完整。CHANGELOG 顶部从 `[Unreleased]` 转为正式 `[0.2.0]` release。

剩余 (v0.3 计划): `examples/design-director-b2b-saas/` 端到端示例。

---

## 附录 E: v0.2 沉淀的写作纪律 (作为 v0.3 起点)

v0.2.0 在 ship 后的 review 中发现一类系统性问题：**"过程废话"占比约 50%**。表现为：

- "Honest Limits" 段每条凑齐 5-6 条，其中半数是免责声明腔（"不替代真专业人士"）或重复其他文件的判断（"不在 EPS 缺失时保证质量"）
- 每个模板末尾的"D3 决策提醒：完全自由"重复 5 遍，是开头"可选脚手架"声明的复制粘贴
- 协议文件里的"协议定位"、"本协议保持窄域"等元话语，对执行者没有增量

清理后 v0.2.1 减少约 50 行废话。但底层问题是写作纪律缺失。

### 元纪律：写任何 "limits / 限制 / 不能 / 不做" 类内容时

每条候选必须通过**三问**才能写入：

1. **改变行为？** 读者读完这条会做出不同选择吗？不会 → 删
2. **重复？** 这条信息在本文件或其他文件已经存在吗？是 → 删
3. **结构装饰？** 这条是为了让 section 不空着才写的吗？是 → 删

三问都通过才保留。

### 补充判定：删空一个段后是否补内容

只有同时满足以下两个条件才补 1-2 条新内容：

- 该 section 在文档结构上必需（删除会让文档断层）
- 真有 1-2 条按三问能通过的内容

否则直接删 section，不要为了凑结构而填充。

### v0.3 起点

新生成任何文件时，按上述纪律执行。**不写满 section**——section 短一句话比写满 6 条废话强。

特别警惕的几个高频废话模式：

- "Honest limits" 段——超过 3 条时检查是否凑数
- "与其他文件的关系" / "协议定位" 段——确认每条引用真的影响执行
- "决策提醒：完全自由" / "本模板不是法律" 类语句——一次声明即可，不要每个文件都重复
- "不替代真专业人士" / "仅作信息辅助" 类——AI 角色通用免责声明，不构成 luban 特有的 honest limit




---

## 附录 F: v0.3.0 内化重构记录 (2026-05-25)

### 触发

用户在 v0.2.1 ship 后审视产物，指出两个根本性问题：

1. **"EPS是v1 现在迭代后是v2 你之前难道一直没搞清楚？为什么还存留什么EPS方法论"**——v0.2.x 的"luban 是 EPS 的执行层"叙事是错的。Nuwa 不依赖任何 runtime skill 就能工作，luban 应该是同样形态。借鉴方法论 ≠ 运行时依赖。
2. **"SKILL.md开头按照claude skill标准格式YAML设计"**——v0.2.x 的 SKILL.md YAML 没核实过 Anthropic 官方规范，凭 EPS 一份文件推测。

### 用户最终决策

| 问题 | 用户选择 |
|---|---|
| 是否内化 EPS 方法论 | 是。luban v2 独立运行，方法论内化为 generation-protocol 自己的内容 |
| SKILL.md YAML 范围 | 按 Anthropic 官方规范完整重写 |
| `depends_on` 字段处理 | **B**：完全删除——luban v2 独立存在，血统只出现在 CHANGELOG/README 叙事 |

### 执行（通过 web_search 核实官方规范）

核实结果（https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices 与 https://code.claude.com/docs/en/skills）：

- `name` 必填，≤64 字符，仅 lowercase / 数字 / 连字符，无 reserved words
- `description` 必填，≤1024 字符，**必须第三人称**（"Processes..." 而非 "Use this skill..."），无 XML 标签
- 可选字段：`disable-model-invocation`、`allowed-tools` 等

v0.2.x SKILL.md 全部 description 都用了**第二人称指令式**——是错的。

### 改动汇总

详见 `CHANGELOG.md` [0.3.0] 条目。要点：

- generation-protocol §0 整段重写为 luban 7 立场（v0.2.x 是 "依赖 EPS" 叙事）
- §4-§8 五个 Stage 全部去 "按 EPS Stage N" 表述
- §13 validation 重新分组为 "A 组（内容质量 4）+ B 组（结构一致性 2）"
- identity-schema.json 删 depends_on 字段（required 数组 + properties 定义都删）
- SKILL.md frontmatter description 重写为第三人称、885 字符
- skill-template YAML 规范段按官方规范完整重写
- README 主线叙事从 "luban 是 EPS 执行层" 改为 "luban 7 立场 + 方法论血统简述"

### 关键判断

**这是 breaking change 级别**，按 SemVer 应该是 minor bump（v0.2.x → v0.3.0），不是 patch。理由：

- v0.2.x 生成的 identity.json 含 `depends_on`，v0.3.0 schema 不再要求该字段
- 历史 identity.json 不再 schema-valid，但 luban 不做 migration（v0.x 阶段不保证 backward compat）
- 方法论叙事的根本性改变会让 v0.2.x 文档读者重新理解项目

### v0.3.0 与 v0.2.x 的根本区别

| 维度 | v0.2.x | v0.3.0 |
|---|---|---|
| 与 EPS 的关系 | "执行层"——运行时依赖 | "方法论血统"——叙事提及，无运行依赖 |
| 用户安装路径 | 必须先 view EPS | 独立 clone + 加载 |
| generation-protocol §0 | "鲁班不是新方法论，是 EPS 的可执行产品化层" | "luban 7 条核心立场" |
| validation 表述 | "EPS 4 check + luban 2 check" | "A 组 4（内容质量） + B 组 2（结构一致性）" |
| identity.json | 含 `depends_on: ["expert-persona-synthesis"]` | 不含 depends_on 字段 |
| SKILL.md YAML | 第二人称、未核实官方规范 | 第三人称、≤1024 字符、按官方规范 |

### v0.4 起点

继承 v0.2.x 沉淀的 6 条决策（D1-D10）和 v0.2.1 沉淀的写作纪律（附录 E）。新增 v0.3.0 的写作纪律：

- **方法论文档不引用外挂依赖**——任何 "按 EPS Stage N" 表述应在 commit 前自检
- **SKILL.md YAML 必须核实 Anthropic 官方规范**——不能凭单一参考文件推测
- **breaking change 走 minor bump**——v0.x 阶段每个 minor 都可能 breaking，patch 仅用于纯修订
