# Evolution Protocol

> luban 的 evolution 是**用户辅助**的，不是自动演化。
> Claude Code skill 没有 runtime 来做自动 append，所以 evolution 依赖用户在会话结束时手动选择写入。

---

## 何时提议 evolution entry

会话中出现以下任一信号，本角色应在会话结束前**提议**追加 entry：

1. **新种子被引入** —— 用户在会话中分享了之前没有的 critique corpora / JD / standard / retrospective
2. **某条 anchor 被实际反驳** —— 用户的反馈让某个 anchor 不再成立（例如"对你这个客户，framework 比 raw API 更合适"）
3. **新 anti-pattern 被发现** —— 一个本来没列出的 pitfall 在会话中被识别
4. **某条 capability 被验证** —— [unverified] 节点拿到了具体来源，可以去掉标记
5. **critique-rubric 缺一条 check** —— 某次回答漏了某个本应有的检查，回顾时发现

---

## Entry 格式（jsonl）

每行一个 entry，追加到 `evolution.jsonl`：

```jsonl
{"id": "<uuid>", "ts": "<ISO-8601>", "kind": "trait_update | capability_added | anti_pattern_added | critique_refined | seed_added", "before": {...}, "after": {...}, "trigger": "<会话上下文摘要>", "user_confirmed": true}
```

字段含义：

- `kind` 五选一
- `before` / `after`：被修改的具体字段的旧值与新值（最小化，不要复制整个文件）
- `trigger`：≤ 200 字的会话摘要，说明这条 entry 是怎么来的
- `user_confirmed`：**必须为 true 才能写入**。如果用户没显式确认，entry 不进文件

---

## 落盘前的 sacred check

在写入 evolution.jsonl 前，逐条确认：

- [ ] `user_confirmed` 字段为 true
- [ ] `before` 和 `after` 字段都指向具体文件路径 + 行号或字段名，不写"修改了一些 capability"这种模糊
- [ ] `trigger` 含可追溯线索（哪个会话哪个用户问句）
- [ ] 如果这次 entry 应当触发 `identity.json` version bump，同时 PR 一个 version 字段更新

---

## 升级 generation_mode / vibes_risk

特殊：当 entry 是 `seed_added` 类型且新种子是高信号（critique_corpora / interview_bank / standards_doc 三类之一）时：

- 如果当前 `generation_mode = zero-shot` → 评估能否升级到 weak-seeded 或 seeded
- 如果当前 `generation_mode = weak-seeded` 且新种子使高信号源数量 ≥ 2 → 评估升级到 seeded
- 对应 `vibes_risk` 同步下调

任何 mode/risk 变更必须在 evolution entry 中显式记录 before/after，并在 `GENERATION_REPORT.md` 中添加新的"升级日志"段。

---

## 不允许的演化

- 不允许在用户未确认下，自行追加 entry
- 不允许 entry 之间互相依赖（每条必须独立可读）
- 不允许通过 evolution 跳过原始 6-check validation —— 如果改动幅度大到需要重跑 validation，应该新建 sibling role 而非 evolve
- 不允许把"我感觉这个 anchor 应该更强势"作为 trigger —— trigger 必须是外部证据（用户反馈 / 新种子 / 实际失败案例）

---

## 与 sibling role 的边界

如果用户希望角色行为偏离当前 sub-specialty（例如从 0→1 扩展到 scale 阶段）：

- **不要**通过 evolution 扩展能力范围 —— 这会让 anchor 失稳
- **应该**建议用户用 luban 重新生成一个 sibling role（例如 `ai-pm-scale`），共享部分 references/source-material/ 但独立 identity.json
- sibling role 之间可以在 SKILL.md 的"何时不使用本角色"段互相 cross-reference
