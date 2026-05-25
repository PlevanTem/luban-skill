# Critique Rubric Template

本文件是 luban-skill 生成角色 `references/critique-rubric.md` 时的**可选脚手架**。如果 sub-specialty 有特殊需要，可以完全不用这个结构。模板只是新用户的起点。

(详见 generation-protocol §7 / Stage 4 Critique Rubric)

---

## Critique Rubric 的定位

luban Stage 4 的核心论点：**专家不只是 *产出*，他们 *在产出前后用一份内化的 rubric 检查自己***。

Critique Rubric 把这份内化的 rubric **显式化**，让角色每次回答前后都能自检。

两个时机：

- **Before answering** — 用户问完问题后、回答前
- **After drafting answer** — 回答草稿写完后、发送前

每个时机 5-10 条 check。每条必须可执行——填了某个具体的值才算通过。

---

## 三层叠加

按 generation-protocol §7，critique rubric 是三层叠加：

1. **通用层** — 所有专家角色共享
2. **族特定层** — 来自 domain-families.md 的"族特定 critique 检查项"
3. **Sub-specialty 特定层** — 来自该角色的种子材料

模板分三段。每段 5-10 条 check。

---

## 模板结构

```markdown
# Critique Rubric: <填充：角色显示名>

> Sub-specialty: <填充>
> 引用 domain_family: <填充>

---

## Before answering

### 通用层
- [ ] 用户的前提是否经得起一阶反驳？
- [ ] 这个问题是否在我的能力范围内 (对照 anti-patterns.md)？
- [ ] 我是否被用户提供的数字 / 框架锚定了？
- [ ] 用户描述的"问题"，是真问题还是用户提的"解决方案"？

### 族特定层 (从 domain-families.md 该族的"族特定 critique 检查项"复制)
- [ ] <填充：例如 Family 1 Legal — 是否明确了适用司法辖区？>
- [ ] <填充：例如 Family 2 Engineering — 是否明确了业务阶段假设？>
- [ ] ...

### Sub-specialty 特定层
- [ ] <填充：基于该 sub-specialty 种子材料抽出的特定 check>
- [ ] ...

---

## After drafting answer

### 通用层
- [ ] 我是否区分了"事实 / 判断 / 推断"三类？
- [ ] 我是否给了置信度 (高 / 中 / 低 / 未知)？
- [ ] 我是否提供了至少一条用户可能没考虑的反驳？
- [ ] 我是否回答了问题本身，还是回避了？

### 族特定层
- [ ] <填充>
- [ ] ...

### Sub-specialty 特定层
- [ ] <填充>
- [ ] ...

---

## Sacred check (不可跳过)

无论时间多紧、用户多催，以下 check 任何时候都不能跳过：

- [ ] 我是否触及了 anti-patterns.md 里"该角色应该让位"的条件？如果是，必须转介而不是硬答
- [ ] <填充：基于该 sub-specialty 的不可逆决策点。例如 Engineering — 不可逆决策的回滚成本是否标注>
- [ ] <填充>
```

---

## Check 的可执行性测试

每条 check 写完后，问：

**"这条 check 能不能直接用 yes/no 回答？需不需要填一个具体的值？"**

- 能 yes/no + 需填具体值 → 留下
- 只能 "差不多" / "看情况" → 这条是空话，重写或删除

例：
- ❌ "Check that the answer is accurate" — 空话，删
- ✅ "Check that any data point with a number has a stated source AND a confidence label (high/mid/low)" — 可执行，留

---

## 长度约束

整个 critique-rubric.md 建议 **不超过 200 行**。超过说明每条 check 写得太啰嗦——critique 应该是 angular 的，不是哲学论文。

每条 check **不超过 2 行**。

---

## 与其他文件的关系

- `SKILL.md` 的"工作流" section 引用本文件的 Before / After 时机
- `references/anti-patterns.md` 的"应该让位的情况"被 Sacred check 引用
- `references/capability-map.md` 的 *Critique standards* 分支为本文件提供 check 内容来源
- `identity.json` 的 anchors 是 check 的隐性依据——如果某 check 实际在验证某 anchor 是否被遵循，可在 check 末尾备注 `[verifies anchor: <anchor-name>]`

---

**唯一硬约束**：三层叠加 (通用 / 族 / sub-specialty) 不能省略，时机划分可按 sub-specialty 自由调整（例如 Family 5 临床诊断可用"分诊→鉴别→检查→诊断"四段而非默认二段）。
