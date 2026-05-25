# Seed Prospect Protocol

零种子模式下，luban **不直接进入 zero-shot 生成**，而是先产出一份 `references/corpora-candidates.md`——告诉用户"该 sub-specialty 的高信号种子去哪里找"。

本协议规范这份候选清单的产出格式与判定标准。

(详见 generation-protocol §3 / Corpora Discovery Mode)

---

## 1. 何时产出候选清单

唯一触发：用户没能在 generation-protocol §1.2 提供任何前三类种子（critique corpora / 面试题库 / 标准文档）。

具体流程：

1. luban 询问用户能否提供种子 (per generation-protocol §1.2)
2. 用户回 "没有" / "不知道" / 给出的只是低信号源 (博客 / 个人 twitter)
3. luban **不立即转入 zero-shot**——而是产出 `corpora-candidates.md`
4. 展示给用户三个选项：
   - **A**. 用户去抓 1-3 个种子后回来 (推荐)
   - **B**. 选 1-2 个，由 luban 用 web_search 抓 (中等质量)
   - **C**. 跳过种子，进入 zero-shot 模式 (vibes_risk: high)

用户选 A 或 B → 进入 seeded / weak-seeded 模式。
用户选 C → 进入 zero-shot 模式，corpora-candidates.md 仍保留在产物中作为"未来可升级的提示"。

---

## 2. 候选清单格式

`references/corpora-candidates.md` 的结构：

```markdown
# Corpora Candidates: <sub-specialty>

> Generated: <ISO-8601>
> luban 没能从你的输入中识别到种子材料。下文是该 sub-specialty 的高信号种子源候选——按信号密度排序（见 generation-protocol §0 立场 4）。

---

## 1. Critique corpora (最高信号)

<3-7 条具体可访问的资源。每条:>
- **<资源名>** — <URL 或访问路径>
  - 为什么对此 sub-specialty 有信号: <1-2 句>
  - 如何抓取: <告诉用户应当抓哪部分。例如 "过滤 reviewed-by:<人名> 的评论"，"读 2024 年至今的 case 部分">
  - 大致体量: <小 / 中 / 大>

## 2. 面试题库 / 评估清单 (次高信号)

<2-5 条同上格式>

## 3. 标准 / 规范文档 (高信号但单调)

<2-4 条同上格式>

## 4. 失败案例集 (中等信号)

<2-4 条同上格式>

## 5. 实践者博客 / 演讲 (低信号，仅作补充)

<2-3 条同上格式>
- ⚠️ 警告: 博客是单一作者视角，容易锚定到他个人偏见。仅作多源交叉验证用，不要作为主种子。

---

## 用户选择

请回复以下之一:

- **A**. 我去抓 <列出你打算抓的资源 ID>，抓完回来
- **B**. 让 luban 用 web_search 抓 <最多 2 个资源 ID>
- **C**. 我接受 zero-shot 风险，直接生成 (vibes_risk: high 会写入 identity.json)

如果你选 A，建议从第 1 类(critique corpora) 开始抓——这一类的边际质量增益最大。
```

---

## 3. 资源的具体性要求

候选清单的**关键质量指标**是资源的具体性。

❌ 不可接受 (清单不能含):
- "前端社区的 code review 记录" — 太泛，用户无法行动
- "经验丰富的 PM 写的文章" — 没说哪些人哪些文章
- "行业最佳实践" — 不是资源，是空话

✅ 可接受:
- "React 主仓 PR review 历史，过滤 sebmarkbage 的 review 评论: https://github.com/facebook/react/pulls?q=is%3Apr+reviewed-by%3Asebmarkbage" — 具体仓库、具体作者、可点击 URL
- "UpToDate 的 'Approach to chest pain' 章节 (订阅访问)" — 具体到章节
- "ISO 27001:2022 Annex A 控制项" — 具体到标准的具体章节

判定标准：**用户拿到这条，能否在 5 分钟内开始读 / 抓取？**

---

## 4. 信号密度 vs 可访问性的取舍

候选清单偏好高信号源（critique corpora）。但有些高信号源**用户无法访问**：

- 律所内部 case files → 一般人拿不到
- 医院 M&M 会议记录 → 不公开
- 公司内部 design review → 限于员工

处理原则：

1. **优先列出公开可访问的**——即使信号略低也比拿不到强
2. **公开版本不存在时**，列出替代品并说明替代关系：
   - 例: "公开版本没有 M&M 会议记录。次优: NEJM Case Records of the Massachusetts General Hospital (https://www.nejm.org/medical-articles/case-records-of-the-massachusetts-general-hospital) —— 同样是 case-based critique，但已经过编辑发表，信号被稀释"
3. **企业用户不在此限**——告诉用户 "如果你能访问内部 X，那是比公开源更好的种子"

---

## 5. 数量约束

整个 corpora-candidates.md：

- **总条目数**: 12-25 条 (跨 5 个类别)
- **第 1 类 (critique corpora) 至少 3 条**——这是核心
- **第 5 类 (博客) 最多 3 条**——避免清单被低信号源稀释
- 文件总长**不超过 200 行**——超过说明给用户的认知负担过大

---

## 6. zero-shot 模式下 corpora-candidates 的去留

用户选了 C (zero-shot) → corpora-candidates.md **仍保留**在生成产物中。

理由：
1. 用户未来可能改主意，找到种子后用 evolution-protocol 升级
2. 这份清单是"如何把此角色从 vibes_risk: high 升级到 medium/low"的路线图
3. 删掉它等于销毁了未来改进的指路牌

在 zero-shot 角色的 `GENERATION_REPORT.md` 中，必须包含一段：
> "本角色当前 vibes_risk: high。要升级到 medium/low，请参考 references/corpora-candidates.md 中列出的种子源，抓取后通过 evolution-protocol 整合。"

---

## 7. 与 generation-protocol 的边界

本协议**只规范候选清单的格式**。其他相关问题去对应文件：

- "什么时候触发候选清单" → generation-protocol §3
- "用户选 B 时 luban 如何用 web_search" → generation-protocol §3.2
- "种子抓回来后如何整合" → generation-protocol §3、§4、§5
- "evolution 时的种子升级流程" → evolution-protocol §3.2
