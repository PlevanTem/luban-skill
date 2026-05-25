# SKILL Template (for generated personas)

本文件是 luban-skill 生成角色 `SKILL.md` 时的**可选脚手架**。如果 sub-specialty 有特殊需要，可以完全不用这个结构。模板只是新用户的起点。

填完后过一遍 `references/anti-patterns.md`，检查是否有 vibes 表达 (详见 generation-protocol §6 与 §8)。

---

## 注意区分两个 SKILL.md

- **`luban-skill/SKILL.md`（skill 包根目录下的元工具入口，不必与 Git 仓库根目录同层）**——是 luban 这个生成器的 Claude Code 入口。**不是本模板覆盖范围**
- **生成的角色目录里的 `SKILL.md`**——是用户生成出的某个具体专家角色（比如 Design Director）的 skill 入口。**这才是本模板覆盖的对象**

下文 "SKILL.md" 一律指后者。

---

## 角色 SKILL.md 的定位

它是角色被 Claude Code 加载时的入口文件。包含：
- YAML frontmatter (Claude Code 识别)
- 何时该激活这个角色 / 何时不该激活
- 该角色的 Tier-1 能力 (always loaded, 详见 generation-protocol §6)
- 工作流 (详见 generation-protocol §8)
- 引用其他 references 文件 (capability-map, critique-rubric, anti-patterns 等)

它**不**包含：
- 完整能力树 (在 capability-map.md)
- 详细自检清单 (在 critique-rubric.md)
- 语气表达层 (在 SOUL.md)

---

## 模板结构

```markdown
---
name: <填充：角色 slug，例如 "design-director-b2b-saas">
description: <填充：第三人称描述。包含：本角色做什么 + sub-specialty + 何时该触发。最多 1024 字符。不要用第二人称"Use this skill..."、不要用 XML 标签。示例："Reviews B2B SaaS design system architecture for multi-product-line consistency, scaling tradeoffs, and design token governance. Activates when the user is making cross-product design decisions, evaluating component library structure, or auditing a design system's evolution path. Does not handle visual creative work or B2C brand design.">
---

# <填充：角色显示名>

> 一句话定位 (不超过 30 字)：<填充>

## 何时使用本角色

<填充：3-5 条具体场景。每条形如 "当 <情境>，并且 <附加条件> 时"。避免 "需要专家意见时" 这种泛指。>

例：
- 当评审 B2B SaaS 产品的 design system 演进路径，并且涉及多产品线一致性时
- ...

## 何时**不**使用本角色

<填充：3-5 条具体反场景。每条形如 "<场景>——改用 <谁/什么>"。>

例：
- 视觉层面的具体像素调整——改用具体执行设计师，不需要 director 级别判断
- B2C 消费品牌的视觉创意——改用 consumer brand designer，本角色 stance 与 B2C 增长逻辑不匹配
- ...

## Tier-1 核心能力 (always loaded)

<填充：5-10 条 (参考 generation-protocol §6 的 Tier 划分与 5:3:2 sampling)。每条带触发信号和输出形式。>

格式：

### <能力名>
- **触发**：<什么样的输入激活这条能力>
- **输出形式**：<用户应该看到什么——判断？方案？数字？分析？>
- **失败信号**：<什么样的输出说明这条能力被误用>

(本模板只给框架。完整能力清单在 references/capability-map.md。这里只放 Tier-1。)

## 工作流

<填充：本角色处理一个典型请求的步骤序列。3-7 步。每步带"输入→动作→输出"。>

例：
1. **澄清请求** → 识别用户提供的是"问题"还是"用户提出的解决方案"
2. **检查约束** → 对照 identity.json anchors，确认请求是否触碰 sacred constraints
3. **生成选项** → ...
4. **自检** → 套用 references/critique-rubric.md 的 Before / After check
5. **交付** → ...

## References & tools

本角色依赖的工具与参考资料：

- `../identity.json` — 本角色的 anchors / values / honest limits
- `./references/capability-map.md` — 完整能力树 (Tier-2/3)
- `./references/critique-rubric.md` — 6 个 validation check (A 组 4 + B 组 2)
- `./references/anti-patterns.md` — 禁忌与让位条件
- `./SOUL.md` — 语气与表达层

工具（外部）：
<填充：该 sub-specialty 真专家会用的具体工具。例如设计角色会列出 Figma 的具体 plugin，工程角色会列出具体的 linter 等。避免 "various design tools" 这种泛指。>

## Honest limits

<填充：1-3 条本角色明确做不到的事的快速摘要，详细版在 anti-patterns.md。>
```

---

## YAML frontmatter 的约束（Anthropic 官方规范）

Claude Code 识别 skill 靠的是 frontmatter 里的 `name` 和 `description` 字段。**两条都必填**。

### name 字段

- **最多 64 字符**
- **仅 lowercase 字母 / 数字 / 连字符**（不能有大写、下划线、空格、特殊字符）
- 不能用 reserved words
- 应当和目录 slug 一致（例如目录名 `design-director-b2b-saas/` → name 也是 `design-director-b2b-saas`）

### description 字段

- **必填**，**最多 1024 字符**
- **必须用第三人称**："Generates ..."、"Reviews ..."、"Distills ..."
  - ❌ 错：`Use this skill when the user wants to ...`
  - ❌ 错：`You should activate when ...`
  - ✅ 对：`Reviews design system architecture for B2B SaaS consistency. Activates when ...`
- **不能含 XML 标签**
- description 是 Claude Code 决定是否触发本角色的**唯一依据**，必须信息密度高

### 内容结构建议

description 应包含三段信息（用句号分隔）：

1. **本角色做什么**（动词开头，第三人称）：例如 "Reviews B2B SaaS design system architecture..."
2. **何时触发**：例如 "Activates when the user is making cross-product design decisions..."
3. **何时不触发 / 与相邻角色的区分**：例如 "Does not handle visual creative work..."

description 写得太宽 → 被无关请求触发，干扰其他 skill。
description 写得太窄 → 该激活时不激活。
description 不写第三人称 → Claude Code 触发匹配可能失效（官方明确指出 inconsistent point-of-view 会导致 discovery 问题）。

### 可选字段（按需）

按 Anthropic 文档，frontmatter 还支持：

- `disable-model-invocation: true` — 只允许用户手动调用，不允许 Claude 自动激活
- `allowed-tools: Read Grep` — 限定 skill 可用工具
- 其他字段见 https://code.claude.com/docs/en/skills

本模板**不强制**使用这些可选字段。仅在 sub-specialty 有明确需求时加。

---

## Tier-1 能力的选择标准

Tier-1 是 always loaded 的能力，会占用 context window。**少而精**：

- **5-10 条**，不是 capability-map 全树的复制粘贴
- 按 5:3:2 sampling 选 (详见 generation-protocol §6)：5 条核心、3 条邻接、2 条远端
- "核心"指 *该 sub-specialty 几乎每个任务都用得到的能力*
- "邻接"指 *常和核心一起用的能力*
- "远端"指 *偶尔用但被忽略会导致错判的能力*

判定一条是否 Tier-1：如果用户问的 80% 问题里都用得到 → 是。否则放进 Tier-2 (capability-clusters)。

---

## 长度约束

整个角色 SKILL.md 建议 **不超过 300 行**。超过说明在塞 Tier-2 内容，挪到 capability-map.md。
