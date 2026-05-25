# Changelog

本项目遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式，版本号按 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

---

## [Unreleased]

### 仓库布局变更（dogfood）

- **skill 包迁移**：`luban-skill/` → `.claude/skills/luban-skill/`。本仓库本身变成可被 Claude Code 项目级自动加载的 dogfood 布局，进入本仓库即可直接召唤鲁班；要装到全局自用，复制 `.claude/skills/luban-skill/` 到 `~/.claude/skills/luban-skill/` 即可。
- **不影响 skill 内部**：`SKILL.md` 与 `references/` 之间使用相对路径，整目录搬迁后内部相对关系不变，`SKILL.md` 无需修改。
- **README/ARCHITECTURE 同步**：仓库根的 `README.md` 项目结构与超链接、`ARCHITECTURE_v0.2.md` 路径勘误段均已更新指向新位置；ARCHITECTURE 中早期决策块内的结构树保留旧路径，作为决策时点的记录（已在勘误段显式说明）。

### 文档与路径（v0.3.0 期间累计）

- **仓库布局（v0.3.0）**：可安装的 Claude Code skill 位于 `luban-skill/`（内含 `SKILL.md` 与 `references/`）；`README.md`、`CHANGELOG.md`、`LICENSE`、`ARCHITECTURE_v0.2.md` 在仓库根目录。下文历史条目里写的「项目根 `SKILL.md`」或裸路径 `references/...`，均指 **skill 包内**路径（即 `luban-skill/` 下，现为 `.claude/skills/luban-skill/`），除非另起一行明确写「仓库根」。
- **超链接**：根目录 `README.md` 指向主流程的链接已改为新位置 `.claude/skills/luban-skill/references/generation-protocol.md`。

v0.4 计划：
- `examples/design-director-b2b-saas/` 端到端示例
- 至少 1 个外部用户独立验证过的角色生成案例（v1.0 门槛）

---

## [0.3.0] — 2026-05-25

### 重大变化：方法论内化重构

v0.3.0 是 **breaking change 级别的方法论重构**。v0.2.x 把 luban 定位为 "expert-persona-synthesis (EPS) 的执行层"——这意味着用户必须先安装 EPS，luban 才能保证质量。v0.3.0 把方法论**内化为 luban 自身**：

- luban 现在**独立运行**，不再要求用户先 view EPS
- 方法论以"7 条核心立场"形式直接写入 generation-protocol §0
- EPS 作为方法论血统在 README 叙事中提及，但不再是运行时依赖

### 同步发现：SKILL.md YAML 不符合官方规范

v0.3.0 修订过程中通过 web_search 核实 Anthropic 官方 SKILL.md 规范，发现 v0.2.x 的 YAML 写法多处不符合官方要求：

- description 用了第二人称指令式（"Use this skill when..."），官方明确要求**第三人称**
- 没核对 description 字符上限（官方 ≤1024）
- name 字段约束（≤64 字符、lowercase + 连字符）没文档化

同步修订（见下文"YAML 规范化"）。

### 改动

**方法论内化（删除外挂依赖叙事）**：
- `SKILL.md` §0 "关键前置依赖 (MUST CHECK FIRST)" 整段删除——不再强制 view EPS
- `SKILL.md` 项目介绍段从 "luban-skill 是 EPS 的可执行产品化层" 改为 "luban 在 Claude Code 里把生成专业角色这件事升级为..."
- `SKILL.md` §6 协作边界删除 expert-persona-synthesis 条目
- `references/generation-protocol.md` §0 整段重写为 **"luban 7 条核心立场"**（拒绝 vibes persona / 拒绝 LLM 凭空生成 / 强制 sub-specialty / 数据源信号密度 / 三层加载 / 5:3:2 sampling / 6-check validation 非选项）
- `references/generation-protocol.md` §4-§8 所有 "按 EPS Stage N" 改为 luban Stage N 自己的语言
- `references/generation-protocol.md` §9 "SOUL.md（鲁班独有，EPS 没有）" 改为 "SOUL.md（人格层）"
- `references/generation-protocol.md` §12 anti-patterns 段去除 EPS / Nuwa 引用
- `references/generation-protocol.md` §13 validation 从 "EPS 4 + luban 2" 改为 "A 组 4 + B 组 2"（内容质量 / 结构一致性）
- `references/capability-map-template.md` 4 处 "EPS Stage 1" 引用改为 "luban Stage 1"
- `references/critique-rubric-template.md` "EPS 通用层" 改为 "通用层"
- `references/skill-template.md` 2 处 "EPS 4 + luban 2" 等引用清理
- `README.md` "方法论说明：什么是 EPS" 整段改写为 "方法论：luban 的 7 条核心立场"，附 "方法论血统" 简短叙事段
- `README.md` 流程图 Step 2 "EPS 5 Stage Pipeline" 改为 "luban 5 Stage Pipeline"，Step 3 "luban 独有增量" 改为 "人格层 + 诚实账单"，Step 4 "EPS 4 check + luban 2 check" 改为 "A 组 4 + B 组 2"
- `README.md` 立项动机段 EPS 引用清理

**depends_on 字段彻底删除**：
- `references/identity-schema.json` 的 `required` 数组删除 `"depends_on"`
- `references/identity-schema.json` 的 `properties` 删除 `depends_on` 字段定义
- `references/identity-schema.json` 其余字段 description 中 4 处 "Per expert-persona-synthesis..." 改为 "Per luban..."
- `references/generation-protocol.md` §10 identity.json 示例 schema 删除 `depends_on` 行

**SKILL.md YAML 规范化（按 Anthropic 官方）**：
- 项目根 `SKILL.md` 的 description 改第三人称（"Distills professional methodology..."），删 expert-persona-synthesis 引用，控制到 885 字符（≤1024 上限）
- `references/skill-template.md` 的 YAML 约束段按官方规范重写：
  - 显式 `name` 64 字符上限 + lowercase / 数字 / 连字符约束 + reserved words 禁用
  - 显式 `description` 1024 字符上限 + 第三人称硬规则 + 反例对比
  - 显式可选字段（`disable-model-invocation`、`allowed-tools`）说明
  - description 内容结构建议（动词开头 / 何时触发 / 何时不触发三段）
- `references/skill-template.md` 的 YAML 示例从第二人称（"<填充：何时触发..."）改为第三人称（含示例 "Reviews B2B SaaS design system architecture..."）

**版本号同步**：
- `README.md` badge 从 v0.2.1 → v0.3.0
- `references/identity-schema.json` `luban_version` 示例从 0.1.0 → 0.3.0

### 沉淀
- `ARCHITECTURE_v0.2.md` 附录 F：v0.3.0 内化重构记录（待加）

### 已知风险（留待 v0.4 验证）
- 方法论内化后，"luban 7 立场"是否真的能在不读 EPS 的情况下自洽传达——需要外部读者验证
- description 字符数 885（接近上限 1024），未来如果要加触发关键词需重新精简
- v0.2.x 已 ship 的角色 identity.json 含 `depends_on` 字段，v0.3.0 schema 不再要求该字段。新生成的角色 schema 合规，但历史 identity.json 不再 schema-valid。如要严格 backward compat，需加 migration

---

## [0.2.1] — 2026-05-25

### 修订
v0.2.0 ship 后用户 review 发现"过程废话"占比约 50%。本版本系统性清理。

### 清理（约 50 行）
- `SKILL.md` §6 "luban 不做的事 (Honest Limits)"——整段删除（6 条全是免责声明腔或重复其他段）
- `SKILL.md` §8 "当前版本"、§9 "References" 表——整段删除（结构装饰，CHANGELOG 链接和 §3 加载顺序已覆盖）
- `README.md` Honest Limits——6 条减为 3 条（删"重依赖 EPS"、"5 族不穷尽"、"Claude Code skill 无状态约束"）
- `references/generation-protocol.md` §15——5 条减为 2 条（保留"种子质量决定上限"和"SOUL 不跨 session"）
- `references/domain-families.md` 末尾 honest limits——4 条减为 1 条（保留"同族 sub-specialty 差异大"，因这条直接影响生成选择）
- `references/capability-map-template.md` §"D3 决策提醒"——压缩为一行硬约束（3 层封顶）
- `references/critique-rubric-template.md` §"D3 决策提醒"——压缩为一行硬约束（三层叠加不可省）
- `references/skill-template.md` §"D3 决策提醒"——整段删除（纯重复开头声明）
- `references/anti-patterns-template.md` §"D7 唯一例外"——压缩为预告句
- `references/evolution-protocol.md` §7 "Evolution 不能做的事"——5 条减为 3 条（删"不保证一致性"、"不验证 trigger_detail 真实性"）
- `references/seed-prospect-protocol.md` §1 "协议定位"——整段删除（对执行者无增量），后续 § 重新编号

### 沉淀
- `ARCHITECTURE_v0.2.md` 附录 E：v0.2 学到的写作纪律（三问法 + 补充判定），作为 v0.3 起点

### 未改
- 5 个模板的"长度约束"段（行数约束真改变填写行为，保留）
- 5 个模板的"与其他文件的关系"段（具体说明什么内容在哪里写，保留）
- anti-patterns-template 内嵌的"❌→✅"反例对比（这是该模板的工作方式本身）

---

## [0.2.0] — 2026-05-25

### 重大变化
- **完全重定位**：luban-skill 不再是"自创方法论"，而是 **expert-persona-synthesis (EPS) 的可执行产品化层**
- **删除纯零种子模式**：取而代之"种子勘探模式"——零种子时主动产出 corpora 候选清单让用户去找
- **强制 sub-specialty**：拒绝"高级 PM"这类粗粒度输入，必须细化到"B2B SaaS PM"级别
- **6 check validation**：EPS 的 4 个 check（stereotype / critique / refusal / trade-off）+ luban 增量 2 个（anchor consistency / 族特定 check）
- **完整方法论文件包**：13 个文件，2876 行，可独立运行

### 新增 (按批次)

**批 1 (基础治理 + 数据规范)**:
- `LICENSE` — MIT
- `CHANGELOG.md` — 本文件
- `ARCHITECTURE_v0.2.md` — 含 D1-D10 决策记录的架构稿 + 三个批次的产出附录
- `references/identity-schema.json` — identity.json 的 JSON Schema draft-07 规范

**批 2 (5 个模板，可选脚手架定位)**:
- `references/soul-template.md` — SOUL.md 脚手架（5 section: Tone/Stance/Brevity/No-go/Push back）
- `references/skill-template.md` — 角色 SKILL.md 脚手架（YAML frontmatter + Tier-1 能力选择）
- `references/capability-map-template.md` — 能力树脚手架（H2/H3/列表三层 + actionability 测试）
- `references/critique-rubric-template.md` — 自检清单脚手架（三层叠加 EPS/族/sub-specialty + Sacred check）
- `references/anti-patterns-template.md` — 反模式脚手架（**唯一嵌反例的模板**，含 ❌→✅ 对比）

**批 3 (流程协议 + skill 入口)**:
- `references/evolution-protocol.md` — 半自动迭代流程（含 entry schema、触发场景、fold 操作）
- `references/seed-prospect-protocol.md` — 零种子勘探规范（corpora-candidates.md 格式与资源具体性要求）
- `SKILL.md` — luban 项目根入口（Claude Code 加载触发器，显式 trigger only）

### 改动 (批 1 进行的)
- `references/generation-protocol.md`
  - §13 Validation: 从 4 check 扩展到 6 check，加上 luban 增量的 anchor consistency check 和族特定 check
  - §14 交付：GENERATION_REPORT 必含项里 "4 个 validation check" 改为 "6 个"
- `README.md`
  - 项目结构：删除 v1 残留的 `decision-heuristics.md`（其内容已被 capability-map.md + critique-rubric.md 覆盖）
  - 项目结构：seed-prospect-template.md 重命名为 seed-prospect-protocol.md（命名一致性：和 evolution-protocol.md 对称）
  - Honest Limits：删除"东亚/欧美 critique corpora 不平衡"一条（不应作为交付物的衡量项）
  - 立项动机：4 check 表述更新为 6 check

### 决策记录
关键决策见 `ARCHITECTURE_v0.2.md` §3（D1-D10）+ 附录 A（用户最终决策）+ 附录 B/C/D（批次产出）。摘要：
- D1: 显式 trigger only（必须说 "luban"/"鲁班"）
- D3: 生成角色 SKILL.md 完全自由，*-template.md 是可选脚手架而非强制模板
- D7 (修订): 仅 anti-patterns 嵌反例，其他 4 个模板不嵌；soul/skill 加"嵌指针"
- D9: MIT license

### 已知风险 (留待 v0.3 验证)
- 完全自由的 SKILL.md (D3 选 C) 让下游批量工具化路径变难
- 零种子用户实际选择 A/B/C 的分布未经测试——如果大部分选 C，luban 的差异化被消解
- evolution.jsonl 的半自动模式依赖用户主动配合——可能在实际使用中被遗忘

---

## [0.1.0] — 2026-05-25 (early draft)

### 立项
luban-skill 立项。初始定位为"通用专业角色生成 Claude Code skill"。

### 初始构想（已在 v0.2 重定位中被部分推翻）
- 输入领域名一键生成（v0.2 改为：必须 sub-specialty + 推荐种子材料）
- 自创方法论框架（v0.2 改为：依赖 EPS）
- 允许零种子模式（v0.2 改为：零种子触发"corpora 候选清单"产出）

### 文件
- `references/generation-protocol.md` v1 起稿
- `references/domain-families.md` v1 起稿（5 个族骨架）
- `README.md` v1 起稿（项目立项叙事）

### 已知问题（在 v0.2 修复）
- 产物生成顺序错误（identity → SOUL → SKILL → capability-map，应该 capability-map 优先）
- 零种子模式自相矛盾（违反 EPS 方法论但未声明）
- sub-specialty 没强制
- 没有显式声明依赖 EPS

---

## 版本约定

- **0.x.y**：API / 文件结构未稳定，breaking change 不保证保留
- **1.0.0**：第一个稳定版，至少要满足
  - 完整方法论文件包（v0.2 完成）
  - 至少 1 个端到端示例（v0.3 计划）
  - 至少 1 个外部用户独立验证过的角色生成案例
