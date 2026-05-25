# Evolution Protocol

luban 生成的角色不是一次性产物。`evolution.jsonl` 是它的迭代账本。

但 **luban 的 evolution 是用户辅助的，不是自动演化**——Claude Code skill 没有 runtime 持续监听会话，无法自动 append entry。下文说明这种半自动模式如何工作。

(详见 generation-protocol §11)

---

## 1. evolution.jsonl 的定位

它是**角色在使用中被发现需要修改时的变更记录**。

- **是**: 用户审核过的、写入文件的、可追溯的修改账本
- **不是**: 自动演化日志、模型自学习、跨会话记忆

文件格式：JSON Lines (每行一条 entry)。初始生成时是**空文件**。

---

## 2. 什么时候应该追加 entry

四种触发场景：

### 2.1 用户反馈
用户使用该角色后明确说"这个角色在 X 类问题上判断错了"或"这个角色应该 *也* 知道 Y"。

### 2.2 失败案例
角色在某次会话中明显违反了 critique-rubric / anti-patterns / anchor。

### 2.3 新种子材料
用户后续找到了原生成时没有的种子（例如更新的法规、新发布的标准、之前没读到的 critique corpora），需要把新种子整合进来。

### 2.4 6 check 任一项失效
quality validation 在生成时通过，但某次实际使用中明显失败。

---

## 3. 半自动流程

luban 在三个时机**主动提议**写入 evolution entry。是否写入由用户决定。

### 时机 A: 会话结束时
角色完成一次任务后，Claude 应当问用户：
> "本次会话有需要写入 evolution.jsonl 的变更吗？例如：发现的判断错误、应当补充的能力、anti-pattern 漏项。"

用户回 "无" → 不写入。
用户给出内容 → Claude 草拟 entry 并展示给用户，用户确认后才写入。

### 时机 B: 用户提供新种子时
用户中途说"我找到了一份新的 critique corpora，请整合进来"——Claude 应当：
1. 读取新种子
2. 判断需要修改哪些文件（capability-map / critique-rubric / anti-patterns）
3. 草拟变更
4. 草拟对应的 evolution entry
5. 全部展示给用户，用户确认后才落盘

### 时机 C: 用户明确触发
用户说 "更新角色" / "迭代这个角色" / "evolution X" → 进入显式迭代模式，按 §4 流程走。

---

## 4. Entry 格式

每行一条 JSON 对象。schema：

```json
{
  "id": "<UUID v4>",
  "ts": "<ISO-8601 with timezone>",
  "kind": "trait_update | capability_added | capability_removed | anti_pattern_added | anti_pattern_removed | anchor_changed | seed_added | critique_refined | failed_validation",
  "scope": "<path: file or section affected, e.g. 'capability-map.md / Domain knowledge / User behavior'>",
  "before": <修改前状态片段>,
  "after": <修改后状态片段>,
  "trigger": "user_feedback | failure_case | new_seed | validation_failure",
  "trigger_detail": "<具体触发情形的描述，1-3 句>",
  "user_confirmed": true,
  "session_id": "<可选：本次会话标识，用于事后追溯>",
  "luban_version": "<生成 entry 时的 luban 版本>"
}
```

**字段说明**：

- `id`: UUID v4。不要用递增整数——多用户协作时会冲突
- `ts`: ISO-8601 含时区。例如 `"2026-05-25T10:30:00+08:00"`
- `kind`: 9 种之一。**不要发明新的 kind**——如果某次变更不属于任一 kind，先扩展本文档再使用
- `scope`: 用 `/` 分隔的路径，标识修改影响的文件和 section
- `before` / `after`: 修改的具体内容。可以是字符串、对象、或数组。允许省略不重要的字段，但**不能省略两个字段中的任一个**——`before` 缺失意味着无法回滚
- `trigger`: 4 种触发源之一
- `trigger_detail`: 描述具体触发情形。**这是 entry 最重要的部分**——未来读者靠它判断这次变更是否合理
- `user_confirmed`: 必须 `true`。luban 不写入 `false`。如果用户没确认，entry 根本不应该被写入

---

## 5. 示例 entry

```jsonl
{"id": "a3f4b2c1-7e89-4d3a-9c8f-1a2b3c4d5e6f", "ts": "2026-05-25T10:30:00+08:00", "kind": "anti_pattern_added", "scope": "anti-patterns.md / 容易犯的错", "before": {"count": 5}, "after": {"count": 6, "new_entry": "在评估技术债时容易把 '代码丑' 等同于 '技术债'——技术债的定义是 '当前实现限制了未来变更的灵活性'，单纯的代码风格问题不构成技术债"}, "trigger": "user_feedback", "trigger_detail": "用户在一次架构评审中指出，角色把一个命名风格不一致的模块标为高优先级技术债，但实际上该模块未来 6 个月不会被修改，不构成真技术债", "user_confirmed": true, "session_id": "2026-05-25-arch-review-01", "luban_version": "0.2.0"}
```

---

## 6. 何时**不**应该追加 entry

- 用户只是抱怨但没给出具体修改方向 → 不写入。逼用户具体化才写
- 单次会话的一次性偏离 → 不写入。除非反复出现
- 用户的偏好和 sub-specialty 共识相悖 → 警告用户，让用户明确选择"我要修改成我自己的版本"才写入
- Claude 自己判断需要修改但用户没确认 → **绝不写入**

---

## 7. Evolution 不能做的事

读者应当避免的误解：

- **不是自动演化**: Claude Code skill 无 runtime，luban 不能在用户不参与的情况下修改角色。所有 entry 都需用户主动确认追加
- **不是记忆系统**: 角色不会自动记住"上次用户说过什么"。每次会话仍从角色文件加载初始状态
- **不能跨用户同步**: 如果多人共用同一个角色文件，他们的 evolution 互相不知道。需要外部协作工具 (Git / 共享存储)

---

## 8. 何时把 evolution 折叠回主文件

evolution.jsonl 不应无限增长。当满足以下任一条件，应当 **fold** (折叠)：

- entry 累计超过 50 条
- 累计修改影响超过 5 个文件
- 用户主动要求 "整理一下这个角色"

Fold 过程：
1. 读取整个 evolution.jsonl
2. 按 scope 分组所有 entry
3. 把每个 scope 的累计变更应用到对应主文件 (capability-map / anti-patterns 等)
4. 提升 `identity.json` 的 `version` (0.1.0 → 0.2.0 → ...)
5. 把 evolution.jsonl 归档到 `evolution-archive/v0.1.0.jsonl`
6. 清空 evolution.jsonl 重新开始

Fold 是用户触发，luban 不自动 fold。

---

## 9. 与其他文件的关系

- `identity.json` 的 `version` 字段由 fold 操作触发递增
- `references/capability-map.md`, `references/anti-patterns.md` 等被 fold 时直接修改
- `GENERATION_REPORT.md` 在 fold 时应当更新，反映当前角色状态
- `SOUL.md` 一般不通过 evolution 修改——SOUL 是人格层，应当保持稳定。如果需要修改 SOUL，建议重新生成角色而不是 evolve
