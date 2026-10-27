# Tiden｜Intent、独立证据与可追溯交付裁决

**官网：** https://tiden.ai/  
**Docs：** https://docs.tiden.ai/  
**Changelog：** https://tiden.ai/changelog  
**首次研究：** 2026-10-19（当时评为 Core；该日 GitHub 日报/产品档案同步失败）  
**再次研究：** 2026-10-27（复核 10/20–10/23 Changelog，补建长期档案）  
**状态：** 历史 Core，维持；**不是 10/27 新增 Core**。  
**Tracking：** 5/5；**Mechanism Novelty：** 3/4；**Evidence：** B（官方技术文档/Changelog 明确，独立端到端误放行测试不足）。  
**模式归属：** #30 Intent as Source of Truth，辅以 #15 Proposed Action、#21 AI Work as Shared Artifact、#27 Context Belongs to Work。**不新增模式编号。**

## 1. 核心判断

**AI 写代码和 AI 写测试可能来自同一次错误理解；测试绿灯不等于原始需求正确实现。** Tiden 把 Intent/Spec、Test、实际 Run Evidence、Gate Verdict、Branch Review 与人的风险接受从单次 Coding Agent 会话中独立出来，作为可追溯的长期软件对象。

**Mechanism Delta：**

```text
Before:
Human Request → Agent writes Code & Tests → Green CI → Agent says Done

After:
Human-approved Intent/Spec (persistent)
  → Agent proposes Code / Tests
  → Actual run evidence linked to Spec + Branch
  → Independent Gate Verdict
  → Human reviews semantics, evidence, waivers and merge
  → Immutable/retained history where supported
```

**重点不是“再加一层 CI”**，而是交付判断从 Agent 的自述转移到跨会话、跨分支的持久需求与证据关系。

## 2. 真实问题、用户与旧工作流

目标用户：使用 Claude Code、Codex 或其他 Coding Agent 的开发者、QA/平台工程团队、需要审计交付质量的产品组织。

例如用户要求“普通账户每分钟最多 60 次请求，超额返回 429”。Agent 错读为 100 次，同时写出满足 100 次的代码和测试，CI 全绿，仍违背用户意图。

旧工作流中，Prompt、任务承诺、测试、代码审查与交付判断容易散落在 Agent 会话、Issue、PR 和 CI 之间。人必须重新拼接“最初要求—改了什么—哪个测试真的证明了什么—为何允许发布”。

## 3. 新工作流和主要对象

```text
Human 确认任务边界、验收承诺
     ↓
Tiden Intent / Spec / Branch
     ↓
Agent 编码、编写测试、提出 Spec 修订
     ↓
本地或 CI 执行测试（真实运行）
     ↓
Tiden 记录 Run、Case、关联 Spec、分支与证据新鲜度
     ↓
Gate 计算 Pass / Blocked / Not Verified / Risk Accepted
     ↓
Human 审查语义 Diff、风险接受、Review Card
     ↓
合并 / 暂停 / 关闭而保留证据
```

**官方公开的长期对象**：Organization、Workspace、Product、Branch、Spec、Test Case、Test Run、Agent Session、Issue、Release、Quality Gate、Risk Acceptance。Specs/Tests/Components 与代码、PR、运行结果存在关系。Tiden CLI、MCP Server、Reporters、REST API 与 GitHub/GitLab 连接为公开文档所述能力。

## 4. Human / AI / Deterministic Software 分工

| 参与者 | 旧分工 | Tiden 提议的新分工 |
|---|---|---|
| Human | 写需求、逐行检查代码/测试、猜测是否完成 | 审核 Intent/Spec、接受风险、决定合并；对原始需求正确性负责 |
| AI | 理解任务、写代码/测试、常常自行宣布完成 | 生成/修订实现与测试，提出证据和修复，但不拥有最终交付裁决 |
| Deterministic Software | 运行 CI、保存日志 | 按 Spec/Branch/Run 关联证据，计算 Gate，保留可审计状态，阻止未验证交付 |
| 外部测试/运行环境 | 测试实际发生处 | 仍负责产生真实可复核的执行证据 |

## 5. Composable Test v2

**1–2 天可拼出：** Spec.md + Agent Hooks + GitHub Actions + Jest/Pytest + 脚本 Gate + PR Review，足以做“Agent 交付前必须跑测试”的 Happy Path。

**但完整机制不等于这个 Demo：**
- Spec 具有独立身份、版本、分支与来源；
- Test 与 Spec 有关系，不能借其他分支的绿色结果证明当前分支；
- 交付证据必须是实际运行结果，不是模型的声明；
- 风险接受和拒绝交付都要保留上下文；
- 代码、需求、测试的历史可以跨 Agent 会话继承。

因此标为 **Composable but Structural**。如果用少量脚本即可长期稳定复制以上全部结构且人力成本更低，则 Tiden 的产品差异应重新下调；**技术实现复杂不是其 Core 理由**。

## 6. 2026-10-23 新证据：失败、拒绝和放弃也是长期工作状态

**官方 Changelog（10/23）可核查：**

1. **Closed Branch**：分支未合并时可以关闭，仍保留 Specs、Links、Review、Run History；停止自动合并和后续 Spec 修改。此前结束未合并分支需要删除及其历史。
2. **Branch-scoped Gate**：会话进度与 Close Gate 采用 Intent Branch 自身最新 Run；main 或其他分支的测试不再影响当前会话判断。
3. **Issue Status Reason**：Issue 的 resolve/ignore/reopen 可附带原因；原因属于当前状态，后续状态变化可替换或清除。**不能把“当前状态有原因”误报为“原因永久不可修改”**。
4. **Duplicate Test Preview**：Merge Preview 能标记 main 上已有测试，但合并仍会带入重复项，需用户手动删除；尚非自动去重。
5. **Working Contract --brief**：只展示相较同一对话上次 Contract 的变更部分；这是上下文/注意力优化，不是新的产品机制。

**新的研究判断：** 不仅成功交付要有证据，**被否决、放弃、暂停、风险接受的过程也应该被当作业务资产保存**。这直接强化 #21/#27/#30，但不足以产生新的 #31。

## 7. 架构事实、推断与未知

**已确认的公开设计/接口：**
- `tiden intent start / progress / close`、`tiden gate check`、`tiden branch close`、`tiden branch merge-preview` 等 CLI 能力；
- MCP 工具和 Reporter 接口；
- Spec/Test/Run/Branch/Issue/Release 关联，基于测试运行的 Gate；
- Changelog 中明确的 10/23 Closed Branch、branch-scoped run、状态原因与重复测试预览；
- 部分 release verdict/traceability 采用冻结快照（官方文档声明）。

**合理推断：** 产品内部使用独立的持久业务对象与审计关系，而非仅靠 Agent 对话历史；保留未合并历史有利于跨会话追溯失败决策。

**未知：** Agent Runtime 实际部署结构、Memory 策略、关联测试的误匹配率、Spec 由 Agent 自动起草时的偏误率、跨仓库复杂项目的真实误放行率、独立第三方验证、净维护成本。官方 Changelog 是厂商一手实现陈述，不等于独立端到端实验。

## 8. 最近邻 Pattern 与真正新增什么

| Pattern | 已有解释 | Tiden 的具体深化 |
|---|---|---|
| #30 Intent as Source of Truth | Intent 是代码之上的持久控制对象 | 把 Spec 绑定真实测试运行、Gate 和 Release |
| #15 Proposed Action | 动作先成为可审核对象 | Merge/waiver/issue disposition 进入人的裁决 |
| #21 AI Work as Shared Artifact | 工作进展可由其他人/Agent 继承 | 未合并或放弃分支仍保留证据和判断 |
| #27 Context Belongs to Work | 上下文归属任务而非 Agent | Branch/Intent/Spec 承载跨会话上下文 |
| #13 Deterministic/Probabilistic Split | AI 生成，软件校验 | Gate 计算与 Agent 自述分离 |

**结论：不新增 Pattern。** Tiden 是 #30 在“独立交付证据与失败状态留存”方向的高价值案例。

## 9. 对 AI 原生研发、本地 Agent 的复刻路径

既有 Agent Runtime、CLI、Git、MCP、SQLite、文件访问已足以开展最小实验，无须复制 Tiden 的 SaaS、图谱 UI 或所有 SDK。

```text
IntentContract
├── intent_id
├── branch_id
├── original_request
├── accepted_spec_revision
├── promises / acceptance_criteria
├── changed_files / test_refs
├── evidence_run_id + commit_sha
├── verdict
├── waiver_reason + approver
├── disposition: merged | closed | abandoned | blocked
└── actor / timestamp / immutable event log
```

**最小可验证不变量**：
- A 分支的测试不能证明 B 分支的交付；
- Spec 变更后旧 Run 必须失效或标记过期；
- 无测试或无运行证据不得显示为 Verified；
- Agent 的“完成”不等于 Gate Pass；
- 关闭未合并任务不删除证据；
- 人工接受风险必须关联具体 Spec、原因与责任人。

**实验指标**：错误放行率、错误阻断率、证据错链率、被遗忘的未完成承诺数、人工复核分钟数、任务关闭后重新调查所需时间。

**不值得抄**：为了验证一项业务规则重建完整测试管理 SaaS；把 Spec 自动生成视为天然可信；以更多 Gate 或仪表盘代替真实质量评估。

## 10. 对应急管理 / B/G 的启发

把“检查单—隐患—整改—复核—督办”当作统一业务对象关系，而不是 Agent 生成的一组文字报告。

例如 AI 根据视频发现隐患并提出闭环：
- Human/规则定义什么证据能证明整改；
- AI 收集照片、台账、视频片段和现场反馈，提出结论；
- Deterministic Software 验证证据归属、版本、权限、时效、责任主体与状态机；
- 专家对正式结论承担责任；若拒绝关闭或接受例外，**拒绝理由和当时证据同样应保留**。

但**软件能证明证据来自某个记录，不等于能证明现场事实真实**。现场复核和专业判断不能被绿色状态标签替代。

## 11. 最大反证与下一步跟踪

1. Agent 自动写的 Spec 也可能错，Gate 可能只是把错误理解固化。
2. Test 与 Spec 的关联可能由 AI 推断产生，出现虚假覆盖。
3. 保存更多历史可能增加人工阅读负担，净用户投入不降反升。
4. Closed Branch 的状态原因可以被替换，不能将其等同于不可篡改审计。
5. 长期可靠性与误放行率尚无独立实测。

**Tracking 5/5，维持历史 Core。** 下次优先找跨仓库真实项目的错误放行对照、Branch Run 误归属复现、Spec Drift 与 Risk Acceptance 的独立验证。

## Findings

### 2026-10-19｜Original Core Finding
- **Claim**：Intent/Spec 与 Agent 代码/测试分离，交付由独立运行证据与 Gate 决定。
- **Evidence**：官方 Docs、CLI/MCP、Product Hunt 开发者讨论。
- **Status**：公开机制确认；独立可靠性未知；产品机制判断为 Core。
- **Supports**：#30、#15。

### 2026-10-27｜Maturation Finding
- **Claim**：未交付/放弃的 Intent Branch 也必须持久保留证据；验证结果应严格归属当前 Branch/Run。
- **Evidence**：[2026-10-23 Tiden Changelog](https://tiden.ai/changelog)。
- **Status**：厂商公开变更 confirmed-as-claim；实际误放行改善 unknown；归入现有 Pattern 的判断 judgment。
- **Supports**：#21、#27、#30。
- **Challenges**：“绿色测试可跨分支复用”和“被放弃的工作可以删除全部上下文”。
- **Daily**：[2026-10-27](../daily/2026-10/2026-10-27.md)。
