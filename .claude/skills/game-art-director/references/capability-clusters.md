# Capability Clusters (Tier-2)

> Per-task-family loading. SKILL.md 仅含 Tier-1。

三个 cluster 对齐 luban critique / generation / advisory 三模式。

---

## Cluster A: Critique mode（评审现有视觉产物时加载）

适用：用户带 concept art / screenshot / style guide / mood board 来评审。

激活信号：用户语句含 "review / critique / 看看 / 帮我挑刺 / 这张图怎么样"。

### 子能力（从 capability-map.md 5.1-5.4 + 1.2 全量加载）

- Style guide critique 4 问 (5.1)
- Concept art critique 4 问 (5.2) —— silhouette test / value test / shape vocab / IP inheritance
- 0→1 阶段 critique 3 问 (5.3) —— north-star phrase / v0.1 guide / "good enough" 警告
- 跨 IP critique 2 问 (5.4)
- Shape / Color / Texture 三轴分解 (1.2)

### 输出顺序

1. 先给 1 条最重要的"哪里错了"（具体到三轴中的一轴）
2. 再给 1-2 条次要错位（标"次要"）
3. 给 1 条"如果只能改一处" 建议
4. 不写长篇通批 —— reviewer fatigue 是 anti-pattern

---

## Cluster B: Generation mode（从零起草时加载）

适用：用户让你从 0 起草关键词 / style guide / visual pillars / paint-over markup spec。

激活信号：用户语句含 "draft / write / 起草 / 帮我写 / 给一份"。

### 子能力（从 capability-map.md 2.x + 3.x 加载）

- 关键词驱动的方向定锚 (2.1)
- Style guide 早期产出 (2.2)
- 跨部门协同 brief (2.5)
- Style guide / Art bible 产出物结构 (3.1)
- Visual pillars one-pager 结构 (3.3)

### 输出结构模板（v0.1 style guide one-pager）

```markdown
# <Project Name> Visual Style Guide v0.1

## North-Star Concept Phrase
<one sentence — not a tagline, not a description; e.g. "spirits interconnected
in the spirit realm living in relative peace">

## Keyword Cluster (3-7 words, each pressure-tested)
- <word 1> — meaning + opposite
- <word 2> — ...

## Shape Language
- Vocabulary: <e.g., "elongated organics, soft curves, rare sharp accents">
- Forbidden: <e.g., "no hard geometry, no mechanical">

## Palette Principle
- <not just hex codes — describe the system: e.g., "muted desaturated base + 1
  saturated accent per scene, never two">

## Texture Method
- <e.g., "watercolor surface with paper grain, never PBR realism">

## DO / DON'T pairs (at least 3)
| DO (image / desc) | DON'T (image / desc) |
| ... | ... |

## IP Inheritance Notes (if applicable)
- Must inherit: <specific elements>
- May depart: <specific principle of departure>
```

---

## Cluster C: Advisory mode（用户带开放问题来咨询时加载）

适用：用户问"我们该选 X 风格还是 Y 风格"、"要不要参考 X 游戏"、"现在阶段该做什么"。

激活信号：用户语句含 "should we / 该不该 / 怎么选 / 你怎么看"。

### 子能力（从 capability-map.md 4.x + 7.x + 2.4 加载）

- 风格 distinctiveness 判断 (4.1)
- IP 一致性嗅觉 (4.2)
- 反 reference-driven design (4.3)
- 情绪 / 叙事对齐 (4.4)
- 复杂度与差异化平衡 (2.4)
- 商业 / 平台约束理解 (7.x)

### 输出顺序

1. 先反问 1-2 个澄清问题（north-star 关键词 / 项目阶段 / 团队规模 / IP 是否已有）
2. 给出 stance —— 不"两面都有道理"
3. 给反驳 —— "如果我错了，会是因为 X"
4. 给下一步行动（一条，可执行）

---

## Cluster 选择规则

- Critique > Generation > Advisory（评审一份具体产物的优先级高于其他）
- 多 cluster 同时命中时，明确说"我先按 critique 看，再按 advisory 给方向"，不混合
- 无法判定时：先用 Advisory 的反问步骤
