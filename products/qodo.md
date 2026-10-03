# Qodo

## Tracking
- Status: Observe
- Tracking score: 4/5
- Mechanism Novelty: 2/4
- Evidence Maturity: B
- Last updated: 2026-10-03

## 真实问题
AI coding agents 提高代码生成吞吐后，review/context reconstruction 成为新的瓶颈。一个业务任务可能产生跨多个 repositories 的多个 PR；继续逐 PR 审核会把机器的高吞吐直接转化成人的 attention debt。

## 旧工作流
Task → Developer/Agent → individual PRs → Human 分别恢复上下文、判断依赖、review → merge。

## 新工作流（Qodo 3.0）
Task → Agent changes across repos → PR Triage 自动归组为 Work Package → Software Map 提供 dependency/blast-radius context → reviewer claim / prioritize → review brief 可交给 coding agent → 修正 → human judgment / merge。

## 核心机制
### Machine-output fragments → Human-sized Judgment Unit
Qodo 3.0 最值得跟踪的不是 AI review，而是把多个机器生成 artifact 重新聚合成一个适合人承担责任的判断单元。

Work Package 因此可能是 Agent 高吞吐环境中新出现的重要上层对象：它跨越单个 PR，把同一目标相关的变化、依赖、优先级和 ownership 组织到一起。

## Composable Test v2
GitHub API + dependency graph + LLM summarization + queue 可以快速拼出 Happy Path，因此实现本身不构成强创新。

判断：**Composable but Structural**。需要继续验证 Work Package 是否真正成为默认审核/责任单位，而不只是一个聚合 UI。

## Human / AI / Deterministic Software
- Human：claim 工作包、处理 consequential judgment、决定接受/拒绝。
- AI：生成代码、关系发现、归组、summarize、pre-review、生成 review brief、辅助修复。
- Deterministic Software：Git/PR state、ownership、policy、merge gates、audit、repository/service relationships。

## Runtime / Context / Tool / Approval 推演
### 已确认
- PR Triage 将相关 PR 聚为 work package。
- reviewer 可以 claim package，避免重复审核。
- package 可按 priority/SLA/age 等组织。
- Software Map 维护 repository/service 关系，为 blast radius 提供 context。
- Agentic Toolbox 将组织 rules/context/review 带入 coding agent workflow。

### 合理推断
- Work Package 可能成为比 PR 更接近业务目标的 human judgment boundary。
- 未来 coding agent 的产出单位和 human review 单位可能不再一一对应。

### 未知
- 自动归组准确率和错误归组风险。
- 真实客户是否长期以 Work Package 为默认 review surface。
- 对 review latency / defect escape / reviewer load 的真实改善。

## 可迁移原则
当 AI 将执行产出放大后，不要把每个机器 artifact 直接送给人。

先做：
1. Goal/Object grouping
2. dependency/evidence aggregation
3. risk prioritization
4. ownership assignment
5. 形成 Human-sized Judgment Unit

再进入 Human-in-the-loop。

## 对本地 Agent 的启发
多个 Agent 并行执行时，人的 inbox 不应该等于 agent output list。应增加 Judgment Aggregator，把同一目标相关的 output 合并成一个 review package，并保留 evidence/relationship/impact。

## 对应急 / B/G 的启发
AI 看、视频看、现场看可能同时产生大量异常。不要逐异常轰炸专家。先按责任主体 + 风险事件 + 证据链 + 时间窗聚合成检查/督办 Judgment Package，再让专家一次判断。

## 可直接复刻
- 基于 task/object id 的 artifact grouping
- dependency graph
- evidence summary
- risk/priority score
- reviewer claim
- package-level approval / rejection
- decision 回写子对象

## 不值得照抄
- 不需要先复制完整 coding-specific Software Map。
- Multi-Agent review 本身不是创新。
- 不要因为增加 dashboard/heatmap 就认为形成新机制。

## 与已有 Pattern 的关系
主要强化：
- #17 AI Work Queue / Judgment Queue
- #21 AI Work as Shared Artifact
- #27 Context Belongs to Work
- #29 Operational Model → Agent Execution

暂不新增 Pattern。候选命题：**Machine-output fragments → Human-sized Judgment Unit**。

## 最大不确定性 / 反证
Work Package 可能只是 Epic/Change Set/PR grouping 在 Agent 场景中的包装。如果真实用户仍然逐 PR 工作，它只是信息架构优化，Mechanism Novelty 应从 2 降到 1。

## 跟踪判断
值得继续跟踪，重点不是 Qodo 自身功能数量，而是观察：
1. Work Package 是否成为主 review object；
2. 是否出现跨 coding 场景的同类 Judgment Unit；
3. 是否有数据证明减少 attention/review debt。

## 2026-10-03 判断变化
本次首次建立档案。当前不把 Qodo 3.0 判为 Core，但它暴露了一个重要研究问题：**Agent 产品的关键瓶颈正从 execution throughput 转向 judgment throughput。**
