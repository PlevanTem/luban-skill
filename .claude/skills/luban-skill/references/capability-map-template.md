# Capability Map Template

本文件是 luban-skill 生成角色 `references/capability-map.md` 时的**可选脚手架**。如果 sub-specialty 有特殊需要，可以完全不用这个结构。模板只是新用户的起点。

(详见 generation-protocol §4 / Stage 1 Capability Taxonomy)

---

## Capability Map 的定位

它是 luban Stage 1 的产物——该角色 *能做什么* 和 *知道什么* 的 3 层树。

它**不**是：
- 工作流 (在 SKILL.md)
- 决策启发法 (融入 critique-rubric.md)
- 禁忌列表 (在 anti-patterns.md)

它**是**：
- 该 sub-specialty 真专家手里的能力清单
- 每条都是 actionable ("can do X" / "knows when Y applies")，不是 descriptive ("interested in Z")
- 每条都能追溯到一个种子源 (critique corpora / 标准 / 失败案例)，无法追溯的标 `[unverified]`

---

## 格式约定 (D4 决策)

- **H2 (`##`)** = 一级分支 (6-12 个)
- **H3 (`###`)** = 二级分支
- **无序列表 (`-`)** = 叶节点
- **3 层封顶**——更深的层级是设计错误，把内容上移或拆分支

---

## 一级分支的典型集合

按 luban Stage 1 建议，6-12 个分支。典型选择：

- Domain knowledge — 该领域的事实知识
- Methods & frameworks — 该领域的方法论
- Tools & artifacts — 工具与产出物
- Judgment & taste — 判断与品味
- Critique standards — 评审标准 (这条尤其重要，是信号密度最高的源——见 generation-protocol §0 立场 4)
- Collaboration & communication — 协作与沟通
- Industry & business context — 行业与商业理解
- Ethics & failure modes — 伦理与失败模式

并非每个 sub-specialty 都用得到全部 8 个。**6-12 个**是范围，按 sub-specialty 实际选。

---

## 模板结构

```markdown
# Capability Map: <填充：角色显示名>

> Sub-specialty: <填充>
> 数据源密度: <填充：seeded / weak-seeded / zero-shot>
> 总叶节点数: <填充：N 条> | [unverified] 节点数: <填充：M 条>

---

## <一级分支 1：例如 "Domain knowledge">

### <二级分支 1.1：例如 "User behavior patterns in B2B SaaS">
- <叶节点 1.1.1>：可以识别 <填充> [来源：<seed source ID>]
- <叶节点 1.1.2>：知道 <填充> 适用的条件 [来源：<seed source ID>]
- <叶节点 1.1.3>：[unverified] <填充>

### <二级分支 1.2：...>
- ...

---

## <一级分支 2：例如 "Methods & frameworks">

### <二级分支 2.1>
- ...

(共 6-12 个一级分支)

---

## [unverified] 节点清单

如果有 `[unverified]` 标记的叶节点，在文档末尾汇总：

- 节点路径：<分支 1 / 二级 1.1 / 叶节点 1.1.3> — 原因：<填充：为什么 unverified>
- ...

这些节点不进入 Tier-1 (always loaded)。如果用户后续提供了对应种子，把 `[unverified]` 标记移除并补来源。
```

---

## 叶节点的可执行性测试

每个叶节点写完后，问：

**"如果我把这条叶节点扔给一个该 sub-specialty 的真专家，他能 *做* 出对应行为吗，还是只能 *点头* 同意它存在？"**

- 能做出行为 → 这条 actionable，留下
- 只能点头 → 这条 descriptive，重写或删除

例：
- ❌ "Understands the importance of accessibility" — 点头型，删
- ✅ "Can identify WCAG AA color contrast failures by checking calculated ratios against 4.5:1 (text) / 3:1 (large text)" — 行为型，留

---

## 来源标注

每条叶节点末尾应有来源 ID，对应 `identity.json` 的 `seed_sources` 数组里的 entry。

格式：`[来源：<seed-source-id>]`

例：
- "Knows when to apply BLAST radius assessment before any database schema change in production" `[来源：seed-3-postmortem-bank]`

zero-shot 模式下，所有叶节点强制标 `[unverified]`。这本身就是 honest limit (详见 generation-protocol §4 末段)。

---

## 长度约束

整个 capability-map.md 建议 **不超过 500 行**。超过说明在塞 Tier-2/Tier-3 详细内容——拆出 `capability-clusters.md` 等附属文件 (详见 generation-protocol §6 Tier 划分)。

---

## 与其他文件的关系

- `SKILL.md` 引用本文件的 Tier-1 子集——5-10 条最核心的叶节点
- `references/critique-rubric.md` 引用本文件的 *Critique standards* 分支——critique standards 直接转为 self-check 项
- `references/anti-patterns.md` 引用本文件的 *Ethics & failure modes* 分支——failure modes 转为 anti-patterns
- `references/source-material/` 存放种子原文，叶节点的来源 ID 指向这里

---

**唯一硬约束**：3 层封顶（luban Stage 1 强制），不要超过。其他都可以按 sub-specialty 自由组织。
