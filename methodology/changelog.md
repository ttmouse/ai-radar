# Methodology Changelog

## v2.1 — 2026-10-06

### Changed
- Discovery 正式拆为两个并行时间窗：**24–72h Fresh Window** + **7–14d Maturation Window**。
- 明确“发布日期 ≠ 研究成熟日期”：产品发布后新增的 Docs、Help Center、Demo、用户反馈、GitHub、访谈、安全/权限文档可以使旧候选重新进入 Core 评估。
- 新增 **Cross-product Evidence** 原则：同一天多个产品指向同一个 Pattern Candidate 时，优先深拆证据最完整者；其余产品作为独立机制证据，不为了数量重复写多个同构 Core。
- Methodology Reflection 从“只能生成 Proposal”调整为：**低风险、可回滚的方法改进可以自主落地并记录 changelog；L1 顶层研究目标不得静默修改。**

### Why
OpenAI dots 9/29 发布时已有 always-on / proactive 叙事，但直到发布后 Help Center 与 safety docs 继续补全 Custom Rules、Auto-review、proactive research restrictions、pause/reset lifecycle，产品机制才足够清楚。如果只扫描 24–72 小时新品，会系统性漏掉“发布时宣传先行、几天后机制证据成熟”的产品。

同一轮中 Microsoft Autopilot 也出现“从 task 到 role/responsibility”的相似方向。若把两者都作为独立 Core 深拆，会增加日报数量但不会增加机制信息，因此把第二个产品转为 Candidate 的 cross-product evidence。

### Expected impact
- 降低对新品发布时间的偏见。
- 提高 Pattern Candidate 的跨产品验证速度。
- 减少同构产品重复占用日报核心位置。
- 允许 Radar 在不改变顶层目标的前提下自主修正低风险研究流程。

---

## v2 — 2026-09-30

第一次系统性自我复盘。

### Changed
- Composable Test 从强否决器改为压力测试。
- 新增 Mechanism Delta。
- 新增 Default Workflow Test。
- 新增 Responsibility Shift Test。
- Pattern 从“发现即编号”改为 Candidate → Evidence → Pattern。
- Pattern 允许 Merge / Split / Deprecate。
- 评分拆分为 Tracking Score、Mechanism Novelty、Evidence Maturity。
- Finding 升级为最小研究单位。
- 每日日报增加 Methodology Reflection。

### Why
v1 已能有效过滤普通 AI wrapper，但长期运行会出现 Pattern 编号膨胀、可组合性误杀产品机制创新、证据成熟度与机制新颖性混淆等问题。v2 的目标是让系统从“筛产品”升级为“持续修正自己的创新判断框架”。
