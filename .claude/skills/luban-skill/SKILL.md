---
name: luban-skill
description: Distills professional methodology into reusable expert role packages for Claude Code. Generates a complete role directory (SOUL.md, SKILL.md, identity.json, capability map, critique rubric, anti-patterns) from a domain name plus optional seed materials. Activates only on explicit invocation by name — "luban" or "鲁班". Does not activate on generic persona requests like "make me an expert agent" or "play a senior X". 
---

# luban-skill

> Nuwa 蒸馏人，鲁班蒸馏专业方法论。
> Luban distills professional methodology — not individuals, not vibes.

luban 在 Claude Code 里把"生成专业角色"这件事从"让 LLM 描述一个专家"升级为"按方法论蒸馏一个可被真专家承认的角色"。完整方法论在 `references/generation-protocol.md`，可独立运行。

---

## 1. 触发条件

luban 仅在用户**明确说出 "luban" 或 "鲁班"** 时激活。不蹭以下泛触发：

❌ 不触发:
- "make me an expert agent"
- "play a senior X for me"
- "扮演一个资深 Y"
- "generate an expert persona"

✅ 触发:
- "use luban to generate a B2B SaaS PM expert"
- "用鲁班蒸馏一个高级税务律师"
- "luban distill <sub-specialty>"
- "用 luban 生成 / 帮我 luban 一个 ... 角色"

如果用户的请求像 "make me a senior X" 但没说 luban：**不要主动 hijack**——用户可能想要其他工具或就地解决。可以告知 luban 存在，但不强行接管。

---

## 2. 执行流程概览

完整流程在 `references/generation-protocol.md`，共 16 个 section（v0.3.0 起含 §13a 交互协议）。

高层步骤分为 **7 个 phase**（与 generation-protocol §13a.1 的 banner 一致）。每个 phase 开始前**必须**显示阶段 banner：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 Phase N / 7 — <phase name>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

7 个 phase（每步都 view 对应 reference 文件，不要凭记忆执行）：

1. **Phase 1: 解析输入 + 族判定 + sub-specialty 收敛** (§1-§2)
2. **Phase 2: 种子勘探** (§3, seed-prospect-protocol.md) — 零种子时产 corpora-candidates.md
3. **Phase 3: Capability Taxonomy** (§4) — 结束时触发**确认门 A**（capability-map preview）
4. **Phase 4: Anchors + persona 命名** (§5, §9.2) — 结束时触发**确认门 B**（persona preview）
5. **Phase 5: Progressive spec + Rubric + Tools** (§6-§8) — SKILL.md / capability-clusters / critique-rubric / retrieval-sources
6. **Phase 6: SOUL.md (9-section v0.3) + anti-patterns + evolution-protocol** (§9-§12)
7. **Phase 7: Validation + Delivery** (§13-§14) — 6 check 全过后触发**确认门 C**（pre-delivery preview）

每个 phase 用 TaskCreate 维护可视进度。状态信号约定（per §13a.3）：🔍 调研中 / 📝 起草中 / 🧪 验证中 / ✅ 通过 / ⚠️ 需确认。

---

## 3. 加载顺序 (which references to view, in what order)

按需 view，不要一次性全 view (会占用过多 context)：

**首次进入 luban 时必 view**:
- `references/generation-protocol.md` — 主流程

**Step 1-2 (输入解析 + 族判定) 时 view**:
- `references/domain-families.md` — 5 个族骨架 + 每族的 critique corpora 候选类型

**Step 3 (零种子时) view**:
- `references/seed-prospect-protocol.md` — 候选清单格式规范

**Step 4-5 (生成产物) 时**:
- 生成 `identity.json` 前 view: `references/identity-schema.json`
- 生成 `SOUL.md` 前 view: `references/soul-template.md`
- 生成 `<role>/SKILL.md` 前 view: `references/skill-template.md`
- 生成 `capability-map.md` 前 view: `references/capability-map-template.md`
- 生成 `critique-rubric.md` 前 view: `references/critique-rubric-template.md`
- 生成 `anti-patterns.md` 前 view: `references/anti-patterns-template.md`

**用户后续要 evolve 角色时 view**:
- `references/evolution-protocol.md`

---

## 4. 关键决策点 (where Claude must stop and ask user)

luban 分两类 stopping points：**stop-and-ask**（用户不答 luban 走不下去）+ **preview-and-confirm**（用户审阅 luban 阶段产物）。

### 4.1 Stop-and-ask 门（6 个，沿用 v0.2）

1. **sub-specialty 不明确** — 用户只给"产品经理"这种粗粒度，必须回问到"B2B SaaS PM"级别 (per generation-protocol §1.1)
2. **族不匹配** — 用户的领域不在 5 个族内 (per domain-families.md "未匹配处理")，让用户选 (a) 自己写族骨架 (b) 自由模式
3. **零种子** — 用户没能提供前三类种子，产出 corpora-candidates.md 后让用户选 A/B/C (per seed-prospect-protocol)
4. **vibes_risk 即将被设为 high** — 进入 zero-shot 模式前，最后一次明确警告并请求确认
5. **6 check 任一失败** — 不能放水通过。回到对应 stage 修复
6. **evolution entry 落盘前** — `user_confirmed` 必须 true 才能写入 evolution.jsonl

### 4.2 Preview-and-confirm 门（3 个，v0.3.0 新增 — per generation-protocol §13a.2）

- **门 A**: capability-map.md 落地后 — show 摘要 + [unverified] 数量，请用户决定 continue / revise / cancel
- **门 B**: anchors + SOUL §1-§5 落地后 — show persona 预览（handle、名字、intro、sacred anchors），请用户决定 continue / rename / revise anchors / cancel
- **门 C**: validation 全过后、写 GENERATION_REPORT 前 — show 交付摘要（路径、模式、honest limits），请用户决定 commit / revise

### 4.3 命名确认（v0.3.0 强制 — per generation-protocol §9.2）

在确认门 B 触发**之前**，luban 必须用 AskUserQuestion 三选一确认 display name：
- 选项 A: 功能名（基于 sub-specialty slug）
- 选项 B: luban 建议的拟人名（短、中性、不带身份信息）
- 选项 C: 用户自定

详细命名规则见 `references/soul-template.md` "命名的决策流程" 段。

---

## 5. 输出产物结构

**默认落盘路径**：`.claude/skills/<role-slug>/`（v0.3.0 起改为 Claude Code 原生 skills 路径）

- 路径决策：每个生成的角色是一个 Claude Code Skill，必须放在 `.claude/skills/` 才能被框架原生发现（`/` 浮动面板可见 + auto-activation）
- `<role-slug>` 建议**短形态**（如 `infra-pm` 而非 `agent-infra-pm`），因为 `.claude/skills/` 路径已隐含"这是一个 agent skill"
- 同一项目下多个角色并列存放在 `.claude/skills/` 下，每个角色一个子目录
- luban-skill 自身也住在 `.claude/skills/luban-skill/`，不冲突
- 自动维护 `.claude/skills/INDEX.md` —— 每次生成新角色 append 一行（slug / display name / family / sub-specialty / vibes_risk）
- 用户显式指定其他路径时遵从用户

成功生成的角色目录:

```
.claude/skills/<role-slug>/
├── SOUL.md
├── SKILL.md                          (角色的 Claude Code skill 入口，不是 luban 本身)
├── identity.json                     (符合 references/identity-schema.json)
├── evolution.jsonl                   (初始空)
├── GENERATION_REPORT.md              (诚实账单)
└── references/
    ├── capability-map.md
    ├── critique-rubric.md
    ├── anti-patterns.md
    ├── corpora-candidates.md         (zero-shot 模式必有)
    └── source-material/              (用户提供的种子原文存档)
```

`GENERATION_REPORT.md` 是**强制产物**，必含 (per generation-protocol §14):
- 使用的种子材料 / 缺失的种子
- 领域族 + sub-specialty
- generation_mode (seeded/weak-seeded/zero-shot) + vibes_risk (low/mid/high)
- 6 个 validation check 的结果
- 已知 honest limits 摘要
- 建议的下一步（如何升级到更高 generation_mode）

---

## 6. 与其他 skill 的协作边界

- **Nuwa**: 用户想蒸馏具体真人 (Munger / Naval / Musk) 时，提示用 Nuwa 不是 luban
- **OpenPersona / clawsouls / soul.md 系列**: 这些做 persona portability，不做方法论蒸馏。如果用户问的是"如何让 persona 跨 platform"，这些更合适
