---
name: agents
description: Lists all luban-distilled expert agents currently installed in this project. Renders a structured roster grouped by domain family, showing slash command, display name, sub-specialty, stage, and vibes_risk for each. Activates when the user asks "which agents do I have", "list my experts", "show available agents", "what luban agents are loaded", "/agents", or similar discovery queries. Does not activate for general "list skills" queries (that's Claude Code's built-in `/` palette), only for luban-distilled expert personas.
---

# `/agents` — Luban Agent Roster

> 浏览本项目所有 luban-distilled expert agents 的入口。读 `.claude/skills/INDEX.md`，按 family 渲染。

## 何时使用本 skill

- 当用户问 "我装了哪些 agent / 专家"、"列一下 luban 蒸馏的角色"、"/agents"
- 当用户问 "有没有 X 领域的 agent" —— 渲染清单后辅助匹配
- 当用户首次进入项目想了解可用专家全集

## 何时**不**使用

- 用户问 "Claude Code 装了哪些 skill" —— 那是 `/` 浮动面板的事，不是本 skill 范围
- 用户问 luban-skill 本身怎么用 —— 跳到 luban-skill
- 用户已经在跟某个具体 agent 对话 —— 不要打断，由用户主动召唤本 skill 才介入

## Workflow

1. **读取 INDEX.md** —— 位于 `.claude/skills/INDEX.md`（项目根相对路径）
   - 不存在 → 友好告知 "尚未蒸馏任何角色，运行 `/luban-skill` 创建第一个"
2. **解析表格** —— 提取 slug / display name / family / sub-specialty / stage / vibes_risk
3. **按 family 分组渲染** —— **只渲染已有角色的 family**。空 family 不显示、不预设、不暗示"建议补充"
   - 跨族角色（如 `product + engineering`）按**主属族**分组，不重复出现
   - family 排序按 INDEX.md "By family" 段的实际顺序
4. **每条 entry 格式**:
   ```
   /<slug>  ·  <Display name>  ·  <sub-specialty 简化>  ·  [<stage>, vibes_risk: <risk>]
   ```
5. **末尾给操作提示** —— 仅 3 条，不附"建议补充哪些 family"：
   - "调用某个 agent：直接输入 `/<slug>` 或在对话中明确召唤"
   - "查看某 agent 的详细身份：读 `.claude/skills/<slug>/SOUL.md` §1-§5"
   - "蒸馏新 agent：`/luban-skill 生成 <领域>`"

⚠️ **不要做的事**：
- 不要列空 family 槽（"Family 1 (Legal): (空 — 待生成)"）—— 5 family 是分类逻辑参考，不是用户清单
- 不要在末尾添加"覆盖空白 / 蒸馏建议 / 推荐补充"—— 这是 preset push，违反 lazy organization 原则
- 不要按某种"完整度"评价当前 roster——angent 数量取决于用户实际需要，不是越多越好

## 输出示例（仅 1 个角色时）

```
你当前装载的 luban 蒸馏角色（共 1 个）：

┌─ Family 4 (Product / Growth / Design) ──────────────────────────┐
│  /infra-pm   Mira the PM    Agent 基础设施 PM    [0→1, medium]  │
└─────────────────────────────────────────────────────────────────┘

调用：直接 `/<slug>` 或对话中召唤 display name
详情：读 .claude/skills/<slug>/SOUL.md §1-§5
新增：/luban-skill 生成 <领域>
```

随用户后续生成更多角色，新 family 会**自动新增**（INDEX.md 按 luban-skill 协议自动维护）。本 skill 只反映 INDEX.md 当前状态。

## 与 luban-skill 的边界

- **luban-skill** 是元工具，*生成* 角色
- **agents** 是 meta-skill，*列举* 已生成的角色
- 两者协同：用户用 luban-skill 蒸馏角色 → INDEX.md 自动更新 → agents 浮动面板显示新角色

## Honest limits

- 本 skill **只列 luban-distilled 角色** —— 不显示其他类型的 Claude Code skill（那是 `/` 面板的事）
- INDEX.md 若被手动破坏 / 格式不规范，本 skill 渲染会降级为"裸 dump 文件内容"，不做容错修复
- 不替你**选**用哪个 agent —— 选哪个让用户自己看 description / SOUL.md 决定
