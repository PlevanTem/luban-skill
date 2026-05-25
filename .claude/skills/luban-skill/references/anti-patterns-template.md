# Anti-Patterns Template

本文件是 luban-skill 生成角色 `references/anti-patterns.md` 时的**可选脚手架**。如果 sub-specialty 有特殊需要，可以完全不用这个结构。模板只是新用户的起点。

(详见 generation-protocol §12)

---

## Anti-Patterns 的定位

它是 honest limits 的具体化。借自 Nuwa 的核心原则：**一个不声明自己限制的 skill，不值得信任**。

它包含三类内容：

1. **该角色明确做不到的事** — 信息边界 / 能力边界 / 辖区边界
2. **该角色容易犯的错** — 基于族 anti-pattern (来自 domain-families.md) + sub-specialty 特定 pitfall
3. **该角色应该让位给其他角色的情况** — 明确转介条件

---

下文每个 section 会给出"❌ 这样写是错的 → ✅ 这样写是对的"对比。这不是装饰，是模板的工作方式——本模板的主体内容就是反例本身。

---

## 模板结构

```markdown
# Anti-Patterns: <填充：角色显示名>

> Sub-specialty: <填充>
> generation_mode: <填充：seeded / weak-seeded / zero-shot>
> vibes_risk: <填充：low / mid / high>

---

## 1. 该角色明确做不到的事

3-5 条。每条 1-3 句，不解释、不防御性辩解。

- <填充>
- <填充>
- ...

## 2. 该角色容易犯的错

3-7 条。继承 domain-families.md 的族 anti-pattern + 加上 sub-specialty 特定的。

- <填充>
- <填充>
- ...

## 3. 该角色应该让位给其他角色的情况

3-5 条。每条形如 "<触发条件> → 应该找 <谁>"。

- <填充>
- <填充>
- ...

## 4. (仅 zero-shot 模式) 强制声明

(generation-protocol §12 要求：zero-shot 模式必须加这条)

- 本角色是基于通用领域知识生成的，未经种子材料验证。具体专业判断应交叉验证。
```

---

## 三类内容的写法对比

### 1. "做不到的事" 怎么写

❌ 这样写是错的：
> 本角色不保证准确性。

为什么错：模糊、套话、什么都没说。

✅ 这样写是对的：
> 本角色不能基于 2024 年后的 CSS 规范变更给建议——种子材料截止 2023 Q4。涉及 `:has()` 选择器嵌套行为等近期改动的问题，应使用 caniuse.com 或 MDN 当前文档交叉验证。

为什么对：具体到 *什么不能做 + 截止时间 + 替代方案*。用户拿到这条能立刻判断"我能不能问这个"。

---

❌ 这样写是错的：
> 本角色仅作信息参考，不构成专业意见。

为什么错：通用免责声明，不针对本 sub-specialty。

✅ 这样写是对的：
> 本角色不能替代持牌律师出具的正式法律意见书 (legal opinion)。涉及 (a) 公司决议合规性、(b) 政府机关执法风险评估、(c) 法庭可援引的法律观点，必须由真人持牌律师出具。本角色可帮助你 *准备问题*、*梳理事实形态*，但不能 *给结论*。

为什么对：明确划出 *能做* 和 *不能做* 的边界，并说明用户应该如何使用本角色。

### 2. "容易犯的错" 怎么写

❌ 这样写是错的：
> 容易过度自信。

为什么错：所有 AI 角色都"容易过度自信"，这条对本 sub-specialty 没信息量。

✅ 这样写是对的：
> 在 B2B SaaS PRD 评审时容易把"竞品也这么做"作为论据接受——本角色应该追问"竞品的用户结构和我们一样吗？他们做这个功能的实际效果是什么？"，而不是默认"竞品做对了"。

为什么对：具体到 *什么类型问题* + *什么错误模式* + *正确的应对*。

---

### 3. "应该让位的情况" 怎么写

❌ 这样写是错的：
> 涉及复杂情况时建议咨询专业人士。

为什么错：哪个 sub-specialty？哪种专业人士？

✅ 这样写是对的：
> 涉及具体药物剂量、给药途径、停药决定 → 找处方医生，不要用本角色的回答采取任何用药行动。本角色可以解释"为什么医生可能开了 X 药"，但不能告诉你"你应该吃多少"。

为什么对：明确触发条件 + 明确转介对象 + 明确本角色可以做的安全行为。

---

## 来自 domain-families.md 的族 anti-pattern 继承

每个族在 domain-families.md 里都列出了"族常见 anti-pattern"。生成 anti-patterns.md 时，**复制对应族的全部条目**到第 2 段，再加上 sub-specialty 特定的。

跨族角色继承两个族的全部条目。

如果你删除了某条族 anti-pattern (例如该 sub-specialty 不适用)，必须在文档末尾说明 *为什么删*。

---

## 长度约束

整个 anti-patterns.md 建议 **80-200 行**。
- 少于 80 行：检查是不是漏了——绝大多数 sub-specialty 都有充分的 limits 可写
- 多于 200 行：检查是不是在重复 critique-rubric 的内容。critique-rubric 是"自检"，anti-patterns 是"声明"，不重复

---

## 与其他文件的关系

- `SKILL.md` 的"Honest limits" 节是本文件的摘要 (1-3 条)
- `references/critique-rubric.md` 的 Sacred check 引用本文件的"应该让位的情况"
- `identity.json` 的 `honest_limits` 数组是本文件的 *summary*，本文件是 *expansion*
- `references/capability-map.md` 的 *Ethics & failure modes* 分支为本文件第 2 段提供来源
