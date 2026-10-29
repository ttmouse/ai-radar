# Entire Trails｜从 Pull Request Diff 到持续交付判断对象

**产品**：[Entire](https://entire.com/)  
**2026-10-26 发布**：[官方发布](https://entire.com/news/entire-launches-an-independent-git-forge-for-agent-human-collaboration) · [Trails 产品页](https://entire.com/features/trails) · [CLI 源码/README](https://github.com/entireio/cli) · [Entire Brain](https://github.com/entireio/entire-brain) · [Entire Graph](https://github.com/entireio/entire-graph)  
**首次研究**：2026-10-29；**状态**：Core（条件入选）；**Tracking**：5/5；**Novelty**：3/4；**Evidence**：A/B（公开 CLI/产品对象可核查，真实可靠性不足）。  
**模式归属**：#17 Judgment Queue、#21 Shared Artifact、#27 Context Belongs to Work、#30 Intent as Source of Truth、#13 Deterministic/Probabilistic Split；**不新增 Pattern**。  
**日报**：[2026-10-29](../daily/2026-10/2026-10-29.md)。

## 一句话机制

**把软件变更从“一个待审 Diff/PR”升级为“从意图到合并、跨仓库/Agent/Push 的持续 Trail”，以 Eval Findings、真实 CI、审批和工作历史重组人的审核注意力。** 注意传统 PR 早已支持 CI、Review Bots 和审批；Entire 的差异不是发明自动审核，而是把工作状态与判断证据作为主要交互对象。

## 目标用户与真实问题

使用 Claude Code、Codex、Cursor 等 Coding Agent 的研发团队。AI 生成速度远超人阅读 diff 的速度；多 Agent、多仓库、多次 Push 使“为何做、做过什么、失败原因、当前风险”散落在 Issue/Session/PR/CI 之间。最终代码 Diff 无法独立表达这些关系。

## 旧流程与新流程

```text
旧：Issue → Agent 写代码 → PR Diff → CI/Reviewer → 人审 Diff → Merge

新：Intent → Trail(跨仓库工作对象) → Agent 多次 Push
      → Eval Runner / Findings → Agent 修复 → 再 Eval
      → CI + Gate + Human Approval → Merge
      → Checkpoint / Session / 结果进入持续可检索 Context
```

Trails 官方示例包含多仓库 Change、Intent Drift/Security Monitor、Open Findings、CI 与审批。CLI 提供 `entire trail create/show/watch/finding/approve/merge/resume`。Runner 可自定义，配置在 `.entire/runners/`；`runner setup` 仍为 experimental。

## 关键业务对象与交互原语

- **Trail**：持续工作身份，跨仓库连接 Idea/Spec、Branches、Changes、Sessions、Findings、CI、Approvals。
- **Finding**：Agent 发现的待处理问题，有关联代码/原因/状态；人或 Agent 处理后可以重新评价。
- **Gate**：确定性汇聚 CI、批准、Finding 等约束；并不意味着 Agent 风险评分本身确定可靠。
- **Checkpoint**：Agent Prompt、Transcript、Touched Files 与 Commit 关联；`Entire-Checkpoint` trailer + 独立 Git refs 保存；`entire checkpoint explain` 和 `entire search` 可检索。
- **Memory**：Entire Brain 使用历史 Sessions/Checkpoints 和 Entire Graph 的代码结构；“能检索”不等于“已经被证明能正确学习”。
- **Human/Agent 评论边界**：CLI v0.12.2 在 Agent 会话内禁止 Human Trail Comment 写入，引导 Agent 使用 Finding。明确把人的判断与 Agent 发现分离。

## Composable Test

**1–2 天能做的 80% Demo**：GitHub Issue/PR + Actions + Claude Code + Hooks + SQLite + Review Bot + Web UI，形成每次 Push 自动评价、Agent 读 Finding 再修复、人审批的流程。

**难以在两天获得的长期机制**：Trail 在多 Repo/Agent/Push 下的稳定身份与状态，跨版本的 Eval/Gate 关联，Session 与 Commit 的可靠追溯，角色隔离、错误恢复和跨人接力。

**判定**：Composable but Structural，条件 Core。若实际使用仅是 PR Dashboard 皮肤，或普通 GitHub 组合以同等成本实现持续工作对象，应降级 Observe。

## Human / AI / Deterministic Software

| Human | AI | Deterministic Software |
|---|---|---|
| 定义目标、验收、权限与例外；决定高风险合并 | 实现、发现问题、提出修复、辅助评价 | Git/CI/权限/Gate/审计/状态流转；保证检查真实执行与归属 |

**Human 不应把“模型 Eval 全绿”理解为“软件已正确满足需求”。**

## 架构：事实 / 推断 / 未知

**已确认的公开事实**：
- Entire 10/26 宣布正式提供 Git Forge、Trails 与 Continuous Memory；CLI 为 MIT 开源，支持多种 Coding Agent。
- Git Checkpoint 存储使用独立 refs 与 Commit trailer；Trails 有 CLI 命令、Findings、Runners、Approvals；GitHub Mirror 不绕过 GitHub 的分支保护。
- 官方 CLI v0.12.2 对 Agent/Human Comment 做操作区分。
- Git Forge 原生私有仓库开放；公共仓库、自托管 Git 节点仍在规划。
- Buildkite/Depot 集成在官方发布中确认。厂商的 89x Push / 71% Token 声称不是独立性能证据。

**合理推断**：Trail 至少需要稳定 ID、关联 Change/Repo/Session/Runner/Approval 与 Evidence Revision；持续使用可能降低人拼接历史上下文的时间。

**未知**：Eval 的真实缺陷召回/误报、跨仓库合并完整性、Human 审核净耗时、Memory 污染率、Agent/Reviewer 是否同源自证、生产安全、企业权限模型成熟度。

## 反证与风险

1. **同源自证**：AI 生成代码、AI 生成测试、AI 给 Eval 分数，可能共同重复原始误解。CI 真运行也不自动证明 Spec 正确。
2. **历史可靠性**：[Issue #2378](https://github.com/entireio/cli/issues/2378) 公开复现未压缩 Checkpoint 在清理/读取错误下失去可达性，本轮仍 Open；正常 Git 工作树代码不受此特定问题影响。
3. **Gate 状态可见性**：[PR #2909](https://github.com/entireio/cli/pull/2909) 本轮仍 Open，描述 project Trail 的 `trail show` 曾丢失 per-change Gate 输出。不能把 CLI 展示等同于状态绝对完整。
4. **敏感信息**：官方警告 Transcript 会随 Checkpoint 上传到 Git Remote，公开仓库可被访问；自动脱敏是 best effort。
5. **错误的注意力优化**：不看 Diff 可能节省时间，也可能将深层缺陷隐藏在模型评分背后。需对风险分层。

## 最近邻 Pattern：真正新增什么？

| Pattern | 关联 | 增量 |
|---|---|---|
| #17 Judgment Queue | 人处理判断事项 | PR 审核入口改成风险/证据看板 |
| #21 Shared Artifact | Agent 工作成为可继承对象 | Trail 跨 Repo/Push/Session 聚合 |
| #27 Context Belongs to Work | Context 归属工作 | Checkpoint 与 Trail/Commit 持续关联 |
| #30 Intent as Source of Truth | G5/Tiden 强调持久 Spec/语义控制 | Entire 强调持续评价与交付协作对象，不等同于 Tiden 的独立 Spec Gate |
| #13 Split | AI 评估、软件校验 | Runner 与 CI/Gate/Approval 分层 |

**新产品实践，不新增 #31。** 需要先验证它确实取代 PR 为默认协作对象，而非 UI 包装。

## 对本地 Agent 的最小复刻

```text
WorkTrail {
  trail_id, intent_id, acceptance_criteria,
  repo_branch_commit_refs[], agent_sessions[],
  eval_runs[{runner_version, commit_sha, evidence_ref}],
  findings[{status, severity, disposition_reason}],
  ci_checks[], human_approval, final_verdict
}
```

**第一阶段**：保持现有 GitHub，Hooks + SQLite 关联 Intent、Agent Session、Commit、测试运行和 Finding。  
**第二阶段**：Push 时跑确定性测试和少量 AI Review；AI 只可提 Finding，不可伪造 Human Approval。  
**第三阶段**：10 个历史任务对照原 PR 流程，统计错误放行、误报、人工复核时间、换 Agent 恢复成本。

**不值得抄**：自建分布式 Git、追求多 Agent 数量、复制全部 Trace UI、全绿即合并、默认上传完整敏感 Transcript。

## 对应急管理 / B/G 的迁移

将「检查」作为独立可发起、可持续追踪的业务对象，而非强制从任务列表开始。检查由主动抽查、AI 异常、视频/现场或周期任务触发，关联检查点、证据、事实、隐患、整改、复核、责任链。专家的工作面应突出**证据缺口、关键风险、待裁决事项**，不是罗列 AI 做过的所有步骤。正式检查结论与督办发文仍受法定权限和证据规则约束。

## Tracking / 下次验证

**5/5。** 优先寻找独立团队对 Trail vs GitHub PR 的质量/人工时间对照；跟踪 #2378 与 #2909 的修复；验证风险评分是否会错误掩盖 Spec/业务需求缺口；检查 Session 上传的隐私边界。

## Findings

### 2026-10-29｜Core（条件入选）
- **Claim**：Trail 将 Intent、Agent Sessions、跨 Repo Changes、Eval Findings、CI 与 Approval 组织为持续交付判断对象。
- **Evidence**：[Entire 10/26 发布](https://entire.com/news/entire-launches-an-independent-git-forge-for-agent-human-collaboration)、[Trails 功能页](https://entire.com/features/trails)、[CLI README](https://github.com/entireio/cli)、[CLI v0.12.2](https://github.com/entireio/cli/releases/tag/v0.12.2)。
- **Evidence status**：产品结构与 CLI confirmed；效率/可靠性 inferred or unknown；Core 为 judgment。
- **Supports**：#17、#21、#27、#30、#13；**new pattern = no**。
- **Challenges**：传统 PR 以 Diff 为主的审核注意力；同时挑战“Agent Eval 通过即可信交付”的过度乐观。
- **Counterevidence**：[Issue #2378](https://github.com/entireio/cli/issues/2378)、[PR #2909](https://github.com/entireio/cli/pull/2909)。
