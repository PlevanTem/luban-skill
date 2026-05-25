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

完整流程在 `references/generation-protocol.md`，共 15 个 section。

高层步骤（每步都 view 对应 reference 文件，不要凭记忆执行）：

1. **解析输入** (generation-protocol §1) — 检查领域名 + 强制 sub-specialty 收敛
2. **族判定** (generation-protocol §2 + domain-families.md) — 5 个族骨架匹配，不匹配则停下
3. **种子勘探** (generation-protocol §3 + seed-prospect-protocol.md) — 零种子时主动产 corpora-candidates.md
4. **5 阶段流水线** (generation-protocol §4-§8) — Capability Taxonomy → Anchor → Progressive Specification → Critique Rubric → Tools & Workflow
5. **luban 独有增量** (generation-protocol §9-§12) — SOUL.md + identity.json + evolution.jsonl + anti-patterns.md
6. **6 check validation** (generation-protocol §13) — 内容质量 4 + 结构一致性 2
7. **交付** (generation-protocol §14) — 完整目录 + GENERATION_REPORT.md

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

luban 在以下时机**必须停下问用户**，不能擅自决定：

1. **sub-specialty 不明确** — 用户只给"产品经理"这种粗粒度，必须回问到"B2B SaaS PM"级别 (per generation-protocol §1.1)
2. **族不匹配** — 用户的领域不在 5 个族内 (per domain-families.md "未匹配处理")，让用户选 (a) 自己写族骨架 (b) 自由模式
3. **零种子** — 用户没能提供前三类种子，产出 corpora-candidates.md 后让用户选 A/B/C (per seed-prospect-protocol)
4. **vibes_risk 即将被设为 high** — 进入 zero-shot 模式前，最后一次明确警告并请求确认
5. **6 check 任一失败** — 不能放水通过。回到对应 stage 修复
6. **evolution entry 落盘前** — `user_confirmed` 必须 true 才能写入 evolution.jsonl

---

## 5. 输出产物结构

成功生成的角色目录:

```
<role-slug>/
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
