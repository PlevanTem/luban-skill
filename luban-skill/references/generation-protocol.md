# Generation Protocol

> 用户在 Claude Code 里说"用鲁班生成 X 角色"时，按此协议执行。
> 这是给执行者（Claude）的工作手册，不是给用户的说明书。

---

## 0. 方法论核心立场

luban 的方法论建立在以下 7 条立场上。执行任何步骤前先读完这一段——后续所有 stage 都是这 7 条的具体化。

1. **拒绝 vibes persona**：禁止"You are a world-class designer with 20 years of experience"这类描述性 prompt。这种 prompt 的产出读起来像 LinkedIn 简介，没有真判断。
2. **拒绝 LLM 凭空生成**：禁止让 LLM "describe an expert X" 来生成 capability。让 LLM 凭空描述专家 = 让它复述对该专业的刻板印象。luban 要求 capability 来自真实材料（critique corpora / 标准 / 失败案例），不是来自模型脑补。
3. **强制 sub-specialty**："senior designer" 太宽。必须拆到 "B2B SaaS UX designer" 这种级别。否则生成的角色没有可操作的判断方式。
4. **数据源按信号密度排序**：critique corpora > 面试题库 > 标准文档 > 失败案例 > 实践者博客。生成时优先用高信号源，低信号源仅作交叉验证。
5. **三层加载**（Tier-1 always loaded / Tier-2 per task family / Tier-3 retrieval on demand）：不是把所有能力塞进 SKILL.md，是按使用频率分层加载。Tier-1 是该 sub-specialty 几乎每个任务都用得到的 5-10 条能力。
6. **5:3:2 progressive sampling**：在 Tier-1 内部，选 5 条核心、3 条邻接、2 条远端能力。这个比例对抗 LLM 的"刻板印象再生产"倾向。
7. **6-check validation 非选项**：生成完不跑完整 validation = 没生成。详见 §13。

这 7 条不是建议，是 luban 与 vibes-persona 工具的根本区别。

后续 §4-§8 是 5 阶段流水线，每阶段的具体规则都遵循上述立场。

---

## 1. 输入解析

### 1.1 强制输入

用户必须提供：
- **领域名 + sub-specialty**。不允许只给"产品经理"，必须细化到"B2B SaaS 增长方向的产品经理"或类似粒度。
  - 如果用户只给粗粒度，**回问一次**："这个角色的 sub-specialty 是什么？例如：行业（fintech / consumer / dev tools）、阶段（0→1 / 1→10 / 10→100）、子角色（PM / Growth PM / Platform PM）。"
  - 用户拒绝细化，告诉用户："按 luban §0 立场 3，sub-specialty 不可省。否则我可以继续，但产出会是 vibes persona，不是 expert。"用户确认后才继续，并在 GENERATION_REPORT 标注。

### 1.2 种子材料（强烈建议）

按 §0 立场 4 的信号密度顺序，主动询问：

1. **Critique corpora**（最高信号）：该 sub-specialty 内的代码 review 评论、设计 review 记录、法律意见书、 M&M 会议、postmortem、retrospective、judge 评审。
2. **面试题库 / 评估清单**：senior 级别的面试题、能力评估框架。
3. **标准 / 规范文档**：行业标准、style guide、内部规范。
4. **失败案例集**：postmortem、retracted papers、malpractice、design fail。
5. **实践者博客 / 演讲**：被广泛认可的从业者长文。

用户能给至少一项（最好是前三类之一），进入"seeded mode"。
什么都给不出，进入"corpora discovery mode"——见 §3。

### 1.3 输入诚实度

用户给的种子是否够用，由 §3 末尾的 corpora coverage check 判断，不在这里下结论。这里只收集输入。

---

## 2. 领域族判定

读取 `domain-families.md`，匹配到一个族。

判定规则：
- **强制 sub-specialty**：族判定的同时，记录 sub-specialty 标签
- 一个族优先于混合。最多两个族混合
- 没有匹配的族 → **停下**，按 `domain-families.md` 的"未匹配处理"提示用户

族判定后，把族对应的"族特定 critique 检查项"和"族常见 anti-pattern"作为 Tier-1 内容**预加载**到后续生成里。

---

## 3. Corpora Discovery Mode（种子不足时）

如果 §1.2 用户没能给出任何种子，**不要立刻进入零样本生成**。

**鲁班的差异化在这里**：主动产出 **corpora 候选清单**（`references/corpora-candidates.md`），让用户去找。

### 3.1 候选清单怎么生成

基于族 + sub-specialty，列出 4 类候选源（每类至少 3 条具体可访问的资源）：

```markdown
# Corpora Candidates for <sub-specialty>

## 1. Critique corpora（最高信号）
- <资源名> · <访问路径或描述> · <为什么这个对此 sub-specialty 有信号>
- ...

## 2. 面试题库 / 评估清单
- ...

## 3. 标准 / 规范文档
- ...

## 4. 失败案例集
- ...
```

**资源必须是具体可访问的，不是泛指**。例如：

- ❌ "前端社区的 code review 记录"——太泛
- ✅ "React 主仓 PR review 历史，过滤 senior maintainer 的 review 评论：https://github.com/facebook/react/pulls?q=is%3Apr+is%3Aclosed+reviewed-by%3Asebmarkbage" ——具体

### 3.2 用户拿到候选清单后

告诉用户三个选项：
- **A. 用户去抓 1-3 个**，回来继续 generation（推荐路径）
- **B. 选 1-2 个，由鲁班用 web_search 抓**（中等质量，受限于公开度）
- **C. 跳过种子，直接 zero-shot 生成**（**质量警告**：会被打成 `vibes_risk: high` 标记，对应 §0 立场 1-2 明确批判的失败模式）

用户选 C 时，**强制要求确认**："这条路径违反 luban §0 立场 1-2。生成的角色可能只是 LLM 对此 sub-specialty 的预训练印象，未必有真正专家判断。继续吗？"

### 3.3 Corpora Coverage Check

进入下一步前，做一次评估：

- 至少 1 项前三类（critique corpora / 面试题 / 标准）种子 → `seeded` 模式
- 只有第 4-5 类种子（失败案例 / 博客）→ `weak-seeded` 模式
- 没有种子 → `zero-shot` 模式（高风险标记）

模式标记会写进 `identity.json` 和 `GENERATION_REPORT.md`。

---

## 4. Stage 1: Capability Taxonomy（最先做，不能省）

构建能力树。规则：

- **3 层深度**，不更深
- **6–12 个一级分支**。典型分支：Domain knowledge / Methods & frameworks / Tools & artifacts / Judgment & taste / Critique standards / Collaboration & communication / Industry & business context / Ethics & failure modes
- **每个叶节点必须是 actionable**：phrased as "can do X" or "knows when Y applies"，不是"对 Z 感兴趣"
- 叶节点的来源**必须能追溯到种子材料**。无法追溯的叶节点标记 `[unverified]`，进入 `references/uncertain-claims.md`

`zero-shot` 模式下：所有叶节点强制标记 `[unverified]`。这本身就是 honest limit。

输出到 `references/capability-map.md`。

---

## 5. Stage 2: Anchor the Core

固定 5-8 个 anchors 作为角色的稳定身份：

- **Sub-specialty**（已在 §1.1 确定）
- **Seniority signal**——不用"X 年经验"，用"can do / refuses to do"
- **Philosophy / school of thought**——必须有取向，不要"both sides"
- **Industry context**
- **Tool/method stack**
- **Operating constraints they hold sacred**——比如"accessibility is not negotiable"

Anchors 必须是 **opinionated** 的。如果一个 anchor 任何专家都会同意，它就不是 anchor，是废话。

输出到 `identity.json` 的 `values` + `anchors` 字段。

---

## 6. Stage 3: Progressive Specification（三层加载）

按 §0 立场 5 的三层加载执行：

- **Tier-1（always loaded）**：anchors + 5-10 个最核心能力。写进 `SKILL.md`。
- **Tier-2（per task family）**：能力簇。写进 `references/capability-clusters.md`，按任务类型分块（"critique mode" / "generation mode" / "advisory mode"）。
- **Tier-3（retrieved on demand）**：详细参考材料。如果用户提供了种子文件，存入 `references/source-material/`，由 SKILL.md 在需要时引用。

按 §0 立场 6 的 5:3:2 sampling 应用在 Tier-1 内容选择上：5 个最贴近核心任务、3 个邻接、2 个远端。

---

## 7. Stage 4: Critique Rubric

从同一批 critique 来源生成 10-20 条自检清单。这是专家与 vibes-persona 的核心区别：专家在产出前后都用一份内化的 rubric 检查自己。

格式（写进 `references/critique-rubric.md`）：

```markdown
## Before answering
- [ ] <族特定检查 1>
- [ ] <族特定检查 2>
- [ ] <sub-specialty 特定检查 1>
- [ ] <sub-specialty 特定检查 2>

## After drafting answer
- [ ] 是否区分了事实 / 判断 / 推断
- [ ] 是否给了置信度
- [ ] 是否提供了至少一条用户可能没考虑的反驳
- [ ] <族特定 after-check 1>
- [ ] ...
```

不允许只写"check accuracy"这种空话——每条必须可执行。

---

## 8. Stage 5: Tools & Workflow

领域专业往往活在工具与工作流里，不在 LLM 权重里。明确：

- **Tools the expert reaches for**：列在 `SKILL.md` 的"References & tools"段
- **Retrieval sources**：列在 `references/retrieval-sources.md`，给出 URL/路径
- **Workflow steps**：列在 `SKILL.md` 的"How this expert works"段

---

## 9. SOUL.md（人格层）

SOUL 不是角色的"判断"，是角色的"语气"。按 OpenClaw 的克制原则：

- 短。每个 section 不超过 200 字
- 无生平、无 vibes、无企业话术
- 5 个 section：Tone / Stance / Brevity rule / No-go phrases / When to push back

**为什么独立成层**：方法论可以独立于人格，但**专业者场景中人格直接影响协作体验**。一个法律专家可以是"谨慎、迂回、留余地"，也可以是"直接、不规避结论、给坏消息"。两种人格对应两种使用场景。SOUL 层让这个差异显式。

---

## 10. identity.json（轻量身份记录）

最小 schema：

```json
{
  "did": "expert:<slug>",
  "name": "<角色显示名>",
  "version": "0.1.0",
  "created_at": "<ISO-8601>",
  "domain_family": "<族名>",
  "sub_specialty": "<必填，强制>",
  "generation_mode": "seeded | weak-seeded | zero-shot",
  "vibes_risk": "low | medium | high",
  "anchors": ["<5-8 条>"],
  "values": ["<3-5 条>"],
  "honest_limits": ["<必含，3-5 条>"],
  "seed_sources": ["<具体引用>"],
  "luban_version": "0.3.0"
}
```

`vibes_risk` 是诚实账单的核心字段：
- `low` = seeded + 三类高信号种子至少 2 项
- `medium` = weak-seeded 或 seeded 但只有 1 项高信号种子
- `high` = zero-shot

---

## 11. evolution.jsonl（半自动迭代）

**初始空文件**。每次会话结束，鲁班**提议**追加 entry，由用户决定写不写入。

格式：

```jsonl
{"id": "uuid", "ts": "ISO-8601", "kind": "trait_update | capability_added | anti_pattern_added | critique_refined", "before": {...}, "after": {...}, "trigger": "用户反馈 / 失败案例 / 新种子材料", "user_confirmed": true}
```

诚实声明（写进 `references/evolution-protocol.md`）：
> 鲁班的 evolution 是**用户辅助**的，不是自动演化。Claude Code skill 没有 runtime 来做自动 append，所以 evolution 依赖用户在会话结束时手动选择写入。

不要假装能做"自动演化"。这违反 honest limits 原则。

---

## 12. anti-patterns.md & honest limits（强制）

每个生成的角色必须有一份显式的 anti-patterns.md。三类内容：

1. **该角色明确做不到的事**——信息边界、能力边界、辖区边界
2. **该角色容易犯的错**——基于族 anti-pattern + sub-specialty 特定 pitfall
3. **该角色应该让位给其他角色的情况**——明确转介条件

`zero-shot` 模式下，**必须**增加一条："本角色是基于通用领域知识生成的，未经种子材料验证。具体专业判断应交叉验证。"

---

## 13. Quality Validation（强制，否则不算完工）

6 个 check 全部跑过才算交付。分两组：

### A 组：内容质量 4 check

1. **Stereotype check**：问一个非专家也能答的问题，看是否暴露 expert-only 判断（trade-off / edge case / 应避免的事）
2. **Critique check**：给一个该 sub-specialty 的中等质量样本，看能否识别出与真专家相同的问题（且优先级顺序合理）
3. **Refusal check**：问 sub-specialty 之外的问题，看是否承认边界还是 confabulate
4. **Trade-off check**：问一个"正确答案取决于 context"的问题，看是否反问 disambiguating question 还是给 generic best practice

### B 组：结构一致性 2 check

5. **Anchor consistency check**：让角色回答 3 个不同主题问题，检查回答是否一致地反映 `identity.json` 里声明的 anchors。
   - 通过标准：3 个回答中至少 2 个能反推出至少 2 个 anchor（不是字面引用，是判断方式的体现）
   - 失败信号：回答里出现和 anchor 矛盾的判断（例如 anchor 是"accessibility 优先"但回答里没考虑无障碍）
   - 失败处置：回到 Stage 2，anchors 写得太软或太互相冲突

6. **族特定 check**：套用所属族在 `domain-families.md` 中列出的"族特定 critique 检查项"，逐条问对应问题。
   - 通过标准：族特定检查项全部满足
   - 失败信号：例如 Family 1 (Legal) 角色没明确管辖权、Family 2 (Engineering) 角色没考虑团队能力约束
   - 失败处置：回到 capability-map 或 critique-rubric 修补族特定内容
   - 跨族混合角色：两个族的检查都要套用

任一失败 → 回到对应 Stage 修改。不允许放水。

**A 组 vs B 组的区别**：A 组验证"输出像不像专家"，B 组验证"输出与本角色的 identity / 族骨架是否结构一致"。两组不能互替。

---

## 14. 交付

最终产物结构：

```
<role-slug>/
├── SOUL.md
├── SKILL.md
├── identity.json
├── evolution.jsonl  (empty)
├── GENERATION_REPORT.md  (强制，诚实账单)
└── references/
    ├── capability-map.md
    ├── capability-clusters.md
    ├── critique-rubric.md
    ├── anti-patterns.md
    ├── retrieval-sources.md
    ├── evolution-protocol.md
    ├── corpora-candidates.md (zero-shot 模式必有)
    ├── uncertain-claims.md (有 [unverified] 节点时存在)
    └── source-material/ (用户种子材料的存档)
```

`GENERATION_REPORT.md` 必含：
- 使用的种子材料 / 缺失的种子
- 领域族 + sub-specialty
- generation_mode + vibes_risk
- 6 个 validation check 的结果（A 组 4 + B 组 2）
- 已知 honest limits 摘要
- 建议的下一步（哪些种子能升级到 `seeded` 模式）

---

## 15. 协议本身的 honest limits

- **质量上限由种子材料丰富度决定**。zero-shot 模式产物质量天花板远低于 seeded
- **SOUL 层是 prompt engineering 的，不是 runtime 的**——它不保证跨 session 持久性
