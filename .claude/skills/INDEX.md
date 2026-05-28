# Luban-distilled Agents Index

> 项目内所有 **luban-skill 蒸馏角色**的清单。luban-skill 在每次生成新角色后自动追加。
>
> `/agents` meta-skill 把这份清单渲染为结构化浮动面板。
> 用户也可直接阅读本文件浏览全集。

---

## All agents

| Slug | Display name | Family | Sub-specialty | Stage | vibes_risk | Version |
|---|---|---|---|---|---|---|
| `infra-pm` | Mira the PM | product + engineering | Agent infrastructure PM (runtime/orchestration/memory/tools/evals) | 0→1 PMF | medium | 0.3.0 |
| `game-art-director` | Vera | product | Game Art Director / Visual Lead (finding visual DNA, v0.1 style guide, first concept reviews) | 0→1 visual定调 | medium | 0.1.0 |

---

## By family

> 只列**已蒸馏**的 family。未占位的不预设——按用户实际生成动态扩展。
> 5 family 骨架（Legal / Engineering / Finance / Product / Clinical）是 luban 分类逻辑参考，不是用户必须填齐的清单。

### Family 4 — Product / Growth / Design
- `/infra-pm` — **Mira the PM** · 0→1 阶段 agent 基础设施的 PM 视角批判者
- `/game-art-director` — **Vera** · 0→1 阶段游戏 Art Director / Visual Lead，关键词驱动的视觉身份建立者

---

## Generation log

| Date | Action | Slug | Note |
|---|---|---|---|
| 2026-05-27 | created | `agent-infra-pm` v0.1.0 | initial weak-seeded generation; path `.claude/agent/agent-infra-pm/` |
| 2026-05-27 | upgraded | `agent-infra-pm` v0.2.0 | persona-fication: 9-section SOUL.md, "Mira the PM", handle `@infra-pm` |
| 2026-05-28 | renamed + relocated | `infra-pm` v0.3.0 | moved to `.claude/skills/infra-pm/` for Claude Code framework integration; slug shortened; slash command `/infra-pm` is real invocation, `@infra-pm` reverts to text-only convention |
| 2026-05-28 | created | `game-art-director` v0.1.0 | weak-seeded; Riot Spirit Blossom + GDC Vault talk titles + senior AD JD signals; display name "Vera" (user picked luban-suggested anthropomorphic name); 6/6 validation pass |

---

## Schema (luban-skill v0.3.0)

每行 entry 包含：
- `Slug` — 目录名 = SKILL.md frontmatter `name` = `/` 命令名
- `Display name` — SOUL.md §1 给的可读名
- `Family` — domain family (5 选 1，或跨族用 `+`)
- `Sub-specialty` — identity.json 的 `sub_specialty` 字段摘要
- `Stage` — 0→1 / 1→10 / 10→100 / 通用
- `vibes_risk` — low / medium / high
- `Version` — semver, 与角色 identity.json `version` 一致

---

## 维护契约

- 本文件由 luban-skill 在生成 / evolve 角色时**自动追加**
- 用户手动编辑此文件**仅限**修正 typo / 调整排序 / 补充 family 分组
- 删除一个角色：从表格移除 + 在 generation log 加一条 `deleted` 行（不删 log，保留历史）

详细维护流程见 `.claude/skills/luban-skill/references/generation-protocol.md` §14。
