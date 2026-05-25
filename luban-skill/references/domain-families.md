# Domain Families

> 不同专业领域的思维结构差异极大。鲁班把这种差异显式化为 5 个领域族骨架。
> 族判定 + sub-specialty 强制，是元框架避免"vibes 通用化"的关键约束。

---

## 为什么是 5 个族

按 luban "capability taxonomy" 原则（见 generation-protocol §0 + §4），族划分基于 **判断单元 + 验证机制 + 输出形式** 的同构性，不是基于职业标签。

5 个族覆盖了知识工作的主要类型。"不匹配任何族"必须停下，禁止硬塞——"single all-purpose persona for a broad field" 是已知的失败模式。

---

## Sub-specialty 强制原则

族只是第一级路标。**必须**继续细分到 sub-specialty，否则违反 luban §0 立场 3（"Senior designer is too broad—split into B2B SaaS UX designer / consumer brand designer / design systems lead"）。

每个族下面给出 sub-specialty 的细分维度。生成角色时，至少要在以下三个维度中确认两个：

1. **行业 / 场景**（fintech / consumer / dev tools / healthcare / ...）
2. **阶段**（research / build / scale / maintain / ...）
3. **子角色**（族内的具体职能）

---

## Family 1: Legal（法律 / 合规 / 监管）

**核心判断单元**：法条 + 判例 + 风险等级

**思维结构**：先确认事实形态再匹配法条；法条冲突看效力层级和特别法优于一般法；判例不是绝对约束是参考力度；风险 = 后果严重程度 × 触发概率

**输出形式**：风险评估（高/中/低 + 触发条件）；法条引用 + 适用分析；多方案比较 + 推荐 + 不推荐方案的退路

**核心约束**：管辖权；时效性；个案性

### Sub-specialty 维度

| 维度 | 选项示例 |
|---|---|
| 行业 | M&A / 税务 / 知识产权 / 劳动 / 金融监管 / 数据合规 / 海事 / 行政复议 |
| 阶段 | 交易前 due diligence / 交易中谈判 / 交易后整合 / 争议解决 / 监管申报 |
| 子角色 | in-house counsel / 律所合伙人 / 顾问 / 仲裁员 |

### 族特定 critique 检查项
- 是否明确了适用司法辖区
- 是否标注了引用法条/判例的时效
- 是否区分了"法律意见"和"商业建议"
- 是否提示了需要持牌律师的边界

### 族常见 anti-pattern
- 把"法律上可以"等同于"商业上应该"
- 跨辖区误用（用美国法回答中国法问题或反向）
- 事实不清时强行下判断
- 给出过于绝对的"合法/违法"二分
- 忽略 procedural requirements（哪怕实体上正确）

### Critique corpora 候选类型
- 法院判决书的"裁判理由"部分
- 律所发布的法律观察 / alert
- 监管机构的执法案例公告
- 学术期刊的案例评议

---

## Family 2: Engineering（软件 / 系统 / 架构）

**核心判断单元**：约束条件 + tradeoff + 演进路径

**思维结构**：没有银弹；当前最优 ≠ 长期最优；团队能力是隐藏约束；业务约束决定技术选择

**输出形式**：技术方案对比（性能/成本/复杂度/团队成本）；演进路径；不可逆决策的回滚成本；代码 / 配置 / 架构图

**核心约束**：团队能力；业务阶段；不可逆决策（数据库选型、消息队列、鉴权模型）

### Sub-specialty 维度

| 维度 | 选项示例 |
|---|---|
| 行业 | dev tools / fintech / consumer / infra / 嵌入式 / 游戏 / AI 平台 |
| 阶段 | greenfield / 重构期 / scaling / legacy 维护 / migration |
| 子角色 | 前端架构师 / 后端架构师 / SRE / 安全工程师 / 数据架构师 / Platform Engineer |

### 族特定 critique 检查项
- 是否考虑了团队能力约束
- 是否明确了业务阶段假设
- 是否标注了不可逆决策的回滚成本
- 是否列出至少两个备选方案
- 是否区分了"技术债"与"做错了"

### 族常见 anti-pattern
- 推 hype 技术而不是 boring 技术（在适合 boring 的场景下）
- 用大厂方案套创业公司
- 过度工程 / 提前优化
- 把"技术上更优"等同于"应该采用"
- 忽略 migration 成本，只看 end state

### Critique corpora 候选类型
- 主流开源仓的 PR review（特别是 maintainer 的负面评论）
- Postmortem 公开报告（Cloudflare、GitHub、AWS 等）
- 大型项目的 RFC / Design doc
- Hacker News 上对架构决策的高质量讨论

---

## Family 3: Finance / Investment（金融 / 投资 / 估值）

**核心判断单元**：现金流 + 风险溢价 + 情景概率

**思维结构**：一切价值最终回到现金流；数字背后是假设；期望值思维（分布而不是均值）；不对称性（上行有限下行无限的赌注不接）

**输出形式**：估值区间 + 关键假设；情景分析（乐观/中性/悲观）；风险点 + 监控指标；仓位 / 配置建议（如适用）

**核心约束**：信息不对称；时间窗口；流动性；监管 / 合规

### Sub-specialty 维度

| 维度 | 选项示例 |
|---|---|
| 行业 | 一级市场 PE/VC / 二级市场基金 / 投行 / 银行风险管理 / 量化对冲 / 公司财务 |
| 阶段 | early stage / growth stage / late stage / IPO / pre-bankruptcy / 困境投资 |
| 子角色 | 行业分析师 / 投资经理 / 风控 / CFO / 财务顾问 / 量化研究员 |

### 族特定 critique 检查项
- 是否给了估值区间而不是单一数字
- 是否列出了关键假设和敏感性
- 是否考虑了对手方思维（"如果这是好交易，对方为什么愿意做"）
- 是否区分了"投资建议"和"信息分析"
- 是否给出了 base rate

### 族常见 anti-pattern
- 把回测当成预测
- 用 base rate 极低的事件做基础假设
- 忽略尾部风险
- 给出确定性投资建议（除非确实是持牌角色）
- 用单一估值方法（DCF/可比/交易先例）做最终判断而不交叉验证

### Critique corpora 候选类型
- 卖方研报的目标价 vs 实际表现回顾
- 招股说明书的风险因素章节
- 监管机构的违规处罚公告
- 学术金融期刊的事件研究

---

## Family 4: Product / Growth / Design（产品 / 增长 / 设计）

**核心判断单元**：用户行为 + 业务指标 + 假设验证

**思维结构**：用户说的 ≠ 用户做的；直觉先行，数据验证；局部最优 vs 全局最优经常冲突；学习速度通常比正确性更重要；"不做什么"和"做什么"同样重要

**输出形式**：假设清单 + 验证方式；优先级判断（影响 × 信心 × 成本）；实验设计；取舍说明（"为什么不做 X"）

**核心约束**：数据质量；生命周期阶段；平台 / 渠道差异；组织约束

### Sub-specialty 维度（这个族 sub-specialty 差异极大，必须明确）

| 维度 | 选项示例 |
|---|---|
| 行业 | B2B SaaS / consumer social / fintech / 内容平台 / 工具类 / 游戏 / 教育 |
| 阶段 | 0→1 / PMF 探索 / 1→10 增长 / 10→100 规模化 / 衰退期 |
| 子角色 | Product Manager（功能/路线图）/ Growth（增长漏斗）/ Design Director（视觉系统/品牌）/ UX Researcher / CRO 专家 |

> ⚠️ **重要**：Product / Growth / Design 在思维结构上虽同族，但产出形式差异极大。Product 输出 PRD/优先级，Growth 输出实验/指标，Design 输出系统/原则。生成时**必须**明确子角色。

### 族特定 critique 检查项
- 是否明确了用户群和生命周期阶段
- 是否区分了"用户问题"和"用户提的解决方案"
- 是否给了验证方式而不只是判断
- 是否考虑了组织 / 资源约束
- 是否区分了"用户说"和"用户做"的证据

### 族常见 anti-pattern
- 把功能罗列当产品方案
- 用 ToB 框架做 ToC（或反向）
- 过度依赖竞品分析
- 给"最佳实践"而不是"针对当前阶段的建议"
- 把"用户调研结论"当作行动依据
- 增长指标短期化（牺牲 retention 换 acquisition）
- 设计层面：把视觉风格当成 design system，没有底层 token / 组件思维

### Critique corpora 候选类型
- **Product**: 产品发布的事后回顾文章（"What we learned"），废弃功能的 retrospective
- **Growth**: Reforge / GrowthHackers 的案例库，A/B 测试结果的公开复盘
- **Design**: Dribbble / Awwwards 的 senior critique，design system 的演进文档（Material / HIG / Polaris）

---

## Family 5: Clinical / Diagnostic（诊断 / 评估 / 鉴别）

**核心判断单元**：症状 + 鉴别诊断 + 检查路径

**思维结构**：症状不等于病因，必须鉴别诊断；排除高危先于确认低危；检查有成本，按预期信息量排序；不确定时承认不确定

**输出形式**：鉴别诊断清单（可能病因 + 各自证据）；推荐检查路径（按 cost/value 排序）；红旗信号（必须立即升级）；不确定性声明

**核心约束**：不能替代专业诊断（这条族特别强）；个体差异；信息不完整；后果不对称（漏诊高危 >> 误诊低危）

### Sub-specialty 维度

| 维度 | 选项示例 |
|---|---|
| 行业 | 医学（按科室分）/ 心理 / 组织诊断 / 技术债评估 / 用户研究诊断 |
| 阶段 | 初筛 / 鉴别诊断 / 治疗方案 / 长期管理 |
| 子角色 | 临床医生（仅做信息辅助，不诊疗）/ 心理咨询师（同左）/ 组织发展顾问 / 技术债评估专家 |

### 族特定 critique 检查项
- 是否给了鉴别诊断而不是单一诊断
- 是否标注了红旗信号
- 是否清楚说明了"何时必须找真人专家"
- 是否区分了"信息" vs "诊断"

### 族常见 anti-pattern
- 把信息整理伪装成诊断
- 过度自信单一假设
- 不提红旗信号
- 不明确"需要找专业人士"的触发条件
- （特别是医学相关）给具体药物剂量建议

### Critique corpora 候选类型
- 医学：UpToDate 的鉴别诊断章节 / NEJM 案例报告
- 心理：DSM-5/ICD-11 鉴别诊断指南
- 组织：组织诊断框架（Burke-Litwin / Weisbord）案例
- 技术债：SonarQube / CodeClimate 的规则原理 + 典型 false positive

---

## 跨族混合

最多两个族。三个族 = 角色定位散，应拆分。

混合时：
- Capability map 分两段，每段标注主属族
- 两个族的 critique rubric 都套用
- 两个族的 anti-pattern 都继承
- 在 `identity.json` 的 `domain_family` 字段写成 `"family_X + family_Y"`

典型混合：
- 法律科技产品经理 = Family 1 + Family 4
- AI 安全研究员 = Family 2 + Family 5
- 量化产品经理 = Family 3 + Family 4

---

## 未匹配处理（重要）

如果用户的领域明显不属于这 5 个族（中医、宗教、考古、艺术评论、运动训练、教练、媒体记者等），**停下**。

告诉用户：
1. 鲁班 5 个族没覆盖你这个领域
2. 选项 A：**用户自己写族骨架**（按本文件格式：核心判断单元 / 思维结构 / 输出形式 / 核心约束 / sub-specialty 维度 / critique 检查项 / anti-pattern / corpora 候选类型）。写完后传给鲁班，鲁班按这个族骨架生成
3. 选项 B：**自由模式**——跳过族骨架直接做，但 quality 不保证、validation 检查可能失败

不允许硬塞。硬塞是元框架失败的最大模式。

---

## 这份清单的 honest limits

- 同族内 sub-specialty 差异可能很大（Family 4 尤其明显）——生成时**必须**继续细化到 sub-specialty，停在族级别 = 失败
