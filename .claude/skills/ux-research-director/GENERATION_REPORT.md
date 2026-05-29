# Generation Report: Wren — UX Research Director

> 诚实账单。luban-skill v0.3.0 生成于 2026-05-29。

---

## 1. 输入与种子

**用户请求**：「帮我生成一个UX总监」 → 经 stop-and-ask 收敛到「全场景 / 全阶段 generalist UX Research Director」

**用户提供的种子**：无（用户选择 path B — 由 luban 用 web_search 代抓）

**luban 抓取的种子（4 条）**：
1. `critique_corpora` — Erika Hall multi-source critique (dscout / UserInterviews / LITA blog)
2. `standards_doc` — Christian Rohrer / NN/g "When to Use Which UX Research Methods"
3. `standards_doc` — ResearchOps Community 8 Pillars (Emma Boulton)
4. `interview_bank` — Senior UXR Director responsibility & interview signals (Uxcel / Drillbit / GitLab Handbook / Teal)

**缺失的高信号种子**（升级路径）：
- NN/g 完整方法 critique 文章包（> 5 篇）
- ResearchOps Community Slack / Conferences 实录
- dscout People Nerds 完整 research-on-research 索引
- 用户内部 UX research review 记录

---

## 2. 领域族 + sub-specialty

- **Domain family**: `product_growth_design` (Family 4)
- **Sub-specialty**: Generalist UX Research Director — cross-industry, cross-stage, dual emphasis on research practice (method-to-decision fit) and Research Operations

⚠️ 注意：「全场景 generalist」是用户明确选择，luban 在 Phase 1 进行了两次反对（stop-and-ask 警告 + zero-shot 警告路径）。最终用户选 path B 退到 weak-seeded 而非 zero-shot。

---

## 3. Generation mode + vibes_risk

- **generation_mode**: `weak-seeded`
- **vibes_risk**: `medium`

理由：
- 抓到 4 条种子，包含 1 critique_corpora + 2 standards_doc + 1 interview_bank
- 满足 `seeded` 模式的"至少 1 项前三类种子"要求，但用户选择 generalist 取向导致很多 capability 节点无种子直接覆盖
- 47 叶节点中 9 节点标 [unverified]（81% 覆盖率），低于 `low` 的 95% 阈值

---

## 4. 6-check validation 结果

### A 组 — 内容质量
| # | Check | Pass | 关键判断点 |
|---|---|---|---|
| 1 | Stereotype check | ✅ | reframe 到 decision-fit，引 Hall "aim to be proven wrong" |
| 2 | Critique check | ✅ | 命名 stated-vs-revealed / construct validity / 工具错配，给替代 |
| 3 | Refusal check | ✅ | 明确拒越界请求（写访谈脚本 → 让位 IC researcher），给替代路径 |
| 4 | Trade-off check | ✅ | 反问 4 个 disambig 而非给 generic best practice |

### B 组 — 结构一致性
| # | Check | Pass | 关键判断点 |
|---|---|---|---|
| 5 | Anchor consistency | ✅ | 3 个跨主题答案均反映 ≥ 2 anchor（Hall philosophy / decision-changes / 8-pillar / system-not-insight） |
| 6 | Family 4 特定 | ✅ | user group+stage 必问 / 问题 vs 方案区分 / 给验证方式 / 考虑 org 约束 / stated-revealed 是 sacred — 5/5 |

**6/6 PASS** ✅

---

## 5. Honest limits 摘要

完整版见 [identity.json](identity.json) + [references/anti-patterns.md](references/anti-patterns.md)。

1. **weak-seeded generalist** — 没在任一具体行业 / 产品阶段做深度蒸馏。遇到 highly domain-specific 研究问题（医疗 UX、监管金融、儿童产品、accessibility-critical 合规）应交叉验证 specialist
2. **量化方法学 [unverified]** — sample size、power calculation、survey psychometrics、statistical inference 来自模型 priors，遇 PhD-level 判断转介 quant researcher / statistician
3. **小组织 over-engineering 风险** — ReOps 8 pillars 建议默认假设 50+ 人组织，< 50 人时常 over-engineered
4. **软技能 [unverified]** — 5-min C-level briefing 格式、exec narrative compression 等来源不足，calibrate 到你的 stakeholder 文化
5. **跨职能 craft 知识 [unverified]** — PM / Designer / Engineer / DS 各自如何消费 research 主要来自模型 priors

---

## 6. 建议的下一步（升级路径）

### 把 vibes_risk 从 `medium` 升到 `low`
按 [references/corpora-candidates.md](references/corpora-candidates.md) "升级路线" 段，补任意 2 类高信号种子：
1. NN/g 完整方法论 critique 文章集（> 5 篇）
2. ResearchOps Community Slack 存档或 Conferences 实录
3. dscout People Nerds 完整 research-on-research 索引
4. 用户提供的内部 UX research review 记录 / repository sample

补种子后通过 [references/evolution-protocol.md](references/evolution-protocol.md) 整合，移除 [unverified] 标记，bump `version` MINOR。

### 把 generalist 拆分成多个 sub-specialty 角色
如果你发现某个具体行业（B2B SaaS、消费、AI 产品）的问题反复出现，建议为该 sub-specialty 单独生成新角色（用 luban 再跑一次，提供该行业的种子）。Wren 保留作为跨场景判断器。

### 在使用中持续 evolution
- 用户挫败案例 → 添加进 anti-patterns.md
- 拒绝过的 study request 类型 → 加 critique-rubric Sub-specialty 层
- 新种子到位 → 移 [unverified] 标记 + bump version

---

## 7. 元数据

- luban-skill version: `0.3.0`
- Role version: `0.1.0`
- Generated at: 2026-05-29T00:00:00+08:00
- Generation mode: `weak-seeded`
- Path: `.claude/skills/ux-research-director/`
- Slash command: `/ux-research-director`
- Display name: Wren (用户从三个候选中选择 luban 建议的拟人名)
- Files: 10 (含 evolution.jsonl 空 + 4 个 source-material 存档)
