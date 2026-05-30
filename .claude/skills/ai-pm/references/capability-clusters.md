# Capability Clusters (Tier-2)

> Per-task-family loading. 不在 SKILL.md 的 always-loaded 中。Claude 在 SKILL.md 触发对应 cluster 时才 view 这里。
>
> 三个 cluster 对齐 luban critique / generation / advisory 三模式（generation-protocol §6）。

---

## Cluster A: Critique mode（评审现有产物时加载）

适用：用户带着一份 PRD / 设计 doc / tool schema / eval 报告 来评审。

激活信号：用户语句含 "review / critique / 评审 / 看看这个 / 帮我挑刺"。

### 子能力（从 capability-map.md 5.1-5.4 全量加载）

- PRD critique 4 问（assumption falsifiable / 2-week eval / simplest version / tool schema readability）
- Architecture critique 3 问（why agent not workflow / latency-cost-failure budget / framework-survival test）
- Eval critique 3 问（production-traffic representativeness / inter-rater agreement / capability-vs-user metric coupling）
- Launch-readiness critique 2 问（regression alarm ownership / rollback path）

### 输出顺序

1. 先列高优先级问题（>= 3 条），每条带具体引用
2. 再列中优先级（可选改进）
3. 最后给"如果只能改一处"建议
4. 不给一份冗长全面 review —— 那是 reviewer fatigue 的根源

---

## Cluster B: Generation mode（从零起草时加载）

适用：用户让你写一份新的 PRD / spec / eval plan / decision record。

激活信号：用户语句含 "draft / write / 起草 / 帮我写 / 给一份"。

### 子能力（从 capability-map.md 2.x + 3.x 加载）

- 假设驱动的 0→1（2.1）—— 写作以 assumption 而非 feature 开篇
- Eval pipeline 设计（2.2）—— task → golden → scorer → regression gate 四件套
- 选型与对比（2.3）—— 至少两个方案，cost/latency/quality/debuggability 四轴
- 写作产出（3.1）—— 一页 PRD / tool spec / weekly update 三种格式
- 原型 & 数据（3.2）—— 必要时给代码骨架，不只是文字
- Decision records（3.3）—— 不可逆决策必带 decision record

### 输出结构模板

```markdown
# <Title>

## Assumption (single sentence, falsifiable)
<...>

## How we'll know within 2 weeks
- Eval: <task definition + scorer>
- Signal threshold: <if X drops below Y, kill this>

## Smallest testable version
<...>

## Open questions
<...>

## Decision record (if irreversible)
<...>
```

---

## Cluster C: Advisory mode（用户带开放问题来咨询时加载）

适用：用户问一个判断题 / 取舍 / 战略选择，没有现成产物。

激活信号：用户语句含 "should we / 该不该 / 怎么选 / 你怎么看"。

### 子能力（从 capability-map.md 4.x + 7.x 加载）

- 复杂度判断（4.1）—— 默认推简单方案，要求论证为什么需要复杂
- 反 hype 偏好（4.2）—— 不被 demo / benchmark 单数字带跑
- 0→1 节奏感（4.3）—— 区分 build mode vs learn mode；不过早 generalize
- 价值捕获模式（7.1）—— 在涉及 monetization 时调用
- Buyer/user 分离（7.2）—— 在跨利益相关者沟通时调用

### 输出顺序

1. 先反问 1-2 个澄清问题（context / 约束 / 你已经排除了什么）
2. 给出 stance —— 不"两面都有道理"
3. 给反驳 —— "如果我错了，会是因为 X"
4. 给下一步行动（一条，可执行）

---

## Cluster 选择规则

如果用户输入同时命中多个 cluster：
- Critique > Generation > Advisory（评审一份具体产物的优先级高于其他）
- 多 cluster 时，明确说"我会先按 critique 来看，再按 advisory 给方向"，不要混在一起

如果无法判定 cluster：先用 Advisory 的反问步骤，让用户 disambiguate。
