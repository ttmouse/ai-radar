# Oracle Fusion Claw

- Status: **Core**
- Tracking Score: **5/5**
- Mechanism Novelty: **3/4**
- Evidence Maturity: **A/B**
- First tracked: 2026-10-05
- Official launch: 2026-09-29

## 一句话判断

Fusion Claw 最值得研究的不是“Oracle 也做 Agent”，而是 Oracle 把企业软件从 **System of Record** 明确推进为 **System of Outcomes**：人不再逐步操作业务流程，而是定义 Outcome、Authority、Accountability 与 Operating Envelope；Agent 负责规划、持续重规划和推进；确定性企业软件负责计算、事务执行、权限约束与审计。

其最重要的新业务对象不是 Agent，而是 **Outcome Run**：一次长期、可重规划、受权限约束、有证据链、有决策记录、有事务执行结果、最终产生 Outcome Receipt 的业务执行实例。

## 真实问题

企业 Agent 真正进入财务、HR、供应链等核心流程时，问题不再是“能否生成建议”，而是：如何让 AI 在数小时/数天的动态业务过程中持续推进目标，同时不把计算精度、权限、政策、审批、审计和责任也交给概率模型。

传统 Copilot 可以给建议；传统 workflow 可以稳定执行预定义步骤；两者都难处理“目标明确、路径动态、需要研究/模拟/优化/重规划、最终还必须执行真实交易”的工作。

## 旧工作流

```text
Human owns process
→ 打开 ERP/HCM/SCM 多个模块
→ 收集数据
→ 分析异常
→ 计算/模拟方案
→ 判断下一步
→ 发起审批
→ 执行事务
→ 环境变化后重新分析
→ 人持续维持整个过程的状态与责任
```

AI 最多作为其中某一步的建议工具。

## 新工作流

```text
Human
→ 定义 Desired Outcome
→ 定义 Operating Envelope
   objectives / SOPs / policies / constraints
   permissions / risk thresholds / decision rights
   approvals / escalation boundaries
        ↓
Outcome Run
        ↓
Frontier Model
reason / plan / adapt / re-plan
        ↓
Deterministic Enterprise Computation
calculate / simulate / optimize / transact
        ↓
Trust Harness
identity / capability / data / action authority
        ↓
必要时 approval / exception escalation
        ↓
继续推进 Outcome
        ↓
Outcome Receipt
```

## Mechanism Delta

**Before:** Application / Workflow / Task 是主要执行对象，人负责维持目标与跨步骤责任。

**After:** Outcome 成为持续执行对象。人定义目标、权限和责任边界；Agent 决定路径；确定性系统执行受约束动作；系统最终为整个 Outcome Run 产生可审计 Receipt。

这不是单纯“Agent 自动做更多步骤”，而是软件的执行单位从 **step/workflow** 上移到 **business outcome under delegated authority**。

## 最关键的产品机制

### 1. System of Outcomes

Oracle 官方明确把 Fusion Applications 的 System of Record 与 Agentic Applications 的 System of Outcomes 区分开。Outcome 不是一句 prompt，而是可持续执行、可重规划的业务目标。

### 2. Enterprise Operating Envelope

目标、SOP、政策、约束、权限、风险阈值、决策权、审批要求、升级边界被组合成 Agent 的确定性运行边界。Agent 可以规划，但不能自己授予权限。

### 3. Probabilistic Planning + Deterministic Execution

Frontier model 只在需要智能判断的地方 reason/plan/adapt；大规模计算和真实交易由 deterministic enterprise computation 执行。这比“让 LLM 驱动所有步骤”更接近生产系统。

### 4. Outcome Trust Harness

每个 Outcome Run 都应用具体 identity、capability、data、action authority。治理不是聊天窗口上的 approval popup，而是执行 Runtime 的组成部分。

### 5. Outcome Receipt

完成后记录 authority、evidence、decisions、actions/transactions 和 result。审计对象从“某次 tool call”升级为“为什么这个业务 Outcome 最终变成这样”。

## Composable Test v2

表面组件可以快速拼：

`Agent Runtime + workflow engine + policy engine + RBAC + approval queue + ERP API + audit log`

可以在 1–2 天做出 Demo。

但无法在 1–2 天等价复制核心机制，因为真正产品对象是贯穿长期执行的 Outcome Run，并要求：持续重规划、企业政策/权限实时约束、概率推理与确定性计算分层、真实事务执行、异常升级、证据/决策/权限完整追踪、最终 Outcome Receipt。

结论：**Composable but strongly structural，进入 Core。**

## Human / AI / Deterministic Software 分工

| Actor | Before | After |
|---|---|---|
| Human | 持有目标，也负责逐步推进、跨系统操作、异常处理 | 定义 Outcome、Authority、Accountability、Guardrails；处理超出 envelope 的判断和例外 |
| AI | 给建议/生成内容 | 研究、规划、模拟、判断、持续重规划并推进 Outcome |
| Deterministic Software | 保存记录、执行人触发的事务 | 承担大规模精确计算、约束验证、权限、事务执行、审计和 Receipt |

核心变化：**Human 从 Process Operator 转向 Outcome Owner / Authority Setter。**

## Runtime / Context / Tool / Approval / Eval

### 已确认事实

- Fusion Claw 是 governed agentic execution runtime。
- 支持 larger-scale / longer-running work、continuous replanning、deterministic execution。
- Operating Envelope 包含 objectives、SOPs、policies、constraints、permissions、risk thresholds、decision rights、approval requirements、escalation boundaries。
- Agent 不能自行授予 authority；需要时请求审批并升级例外。
- Outcome Receipt 记录 authority、evidence、decisions、actions/transactions 和 result。
- 25 个 Claw-powered applications 已上线，覆盖 Ledger、Workforce Staffing、Shipping Consolidation、Account Territory Growth Plan 等。

### 合理推断

- Outcome Run 很可能需要持久状态存储，而不是依赖 conversation history。
- Context 应以 Outcome 为中心组合业务记录、政策、SOP、证据和中间决策。
- Trust Harness 实际承担类似 policy enforcement point / execution gate 的作用。

### 未知

- Outcome Run 的内部 state schema 与恢复机制。
- Frontier model 与 deterministic planner/optimizer 的具体切换协议。
- Eval 如何决定 Outcome 质量、何时自动执行、何时升级。
- 长周期运行的真实成功率、人工介入率和失败恢复率。

## 与现有 Pattern 的关系

它强化：

- #03 Goal → Autonomous Work
- #13 Deterministic / Probabilistic Split
- #14 Agent Workspace + Governance Layer
- #15 Proposed Action as First-class Object
- #17 Judgment Queue
- #22 Agent as First-class Identity
- #26 Agent-facing System of Record
- #27 Context Belongs to Work
- #29 Operational Model → Agent Execution

但它把这些此前分散的机制收敛到了一个更高层的一等对象：**Outcome Run**。

当前不立即创建 #31。按照 Methodology v2，先记录 Pattern Candidate：

> **Outcome as First-class Execution Object / System of Outcomes**

需要继续寻找至少两个独立产品，确认 Outcome 是否真的成为 Agent-native enterprise software 的稳定对象，而不只是 Oracle 的产品命名。

## 可迁移产品原理

1. **持久的是 Outcome，不是 Agent。** Agent/模型可以替换，业务 Outcome 继续存在。
2. **Autonomy 必须绑定 Authority。** “能做”与“被授权做”必须分开。
3. **Reasoning 与 Execution 分层。** 概率模型决定模糊路径，确定性系统负责精确计算和真实事务。
4. **Audit 的单位应该跟责任单位一致。** 如果人委托的是 Outcome，最终审计也应该回答整个 Outcome 为什么这样完成，而不是只留下 tool logs。
5. **Human-in-the-loop 应该放在 authority boundary / exception，而不是每一步。**

## 对本地 Agent 的启发

如果已有 Agent Runtime、CLI、Browser、MCP、本地文件访问，最值得复刻的不是 Oracle UI，而是新增一个持久对象：

```yaml
outcome:
  goal:
  owner:
  authority:
  policies:
  constraints:
  evidence:
  current_state:
  plan:
  decisions:
  executed_actions:
  waiting_conditions:
  exceptions:
  completion_criteria:
  receipt:
```

Runtime 每次唤醒 Agent 都围绕 Outcome 恢复状态，而不是依赖同一 conversation。

## 对应急 / B/G 产品的启发

现有“周期任务 → AI看/视频看/现场看 → 隐患 → 督办 → 验收”可以进一步抽象为一个 Outcome：

> **在本周期内，使责任主体达到可接受风险状态，并形成完整证据与责任闭环。**

AI 可以动态选择检查手段、发现证据缺口、发起补充检查、生成督办、追踪整改；确定性系统守住企业权限、文书签发、期限、风险阈值和审计；专家只处理高风险判断、例外和授权边界。

这比把 AI 分散塞进每个页面更接近真正的 AI-native 重构。

## 可以直接复刻 / 不值得抄

### 可以直接复刻
- Outcome persistent state
- Operating Envelope schema
- deterministic action gate
- exception / approval queue
- Outcome Receipt
- reasoning vs deterministic execution routing

### 不值得抄
- Oracle 特定 ERP 模块划分
- 为了“全自动”强行消灭所有人工判断
- 把 Outcome 当成一个新的营销名称而没有持久状态/权限/证据/receipt

## 最大未知与反证

最大的反证是：**System of Outcomes 可能只是 Oracle 对“long-running governed workflow + agent”的重新包装。**

如果实际使用中 Outcome 仍然是固定 workflow，只在局部节点调用 LLM，那么它的 Mechanism Novelty 应从 3 降到 2。

真正需要后续验证的是：路径是否会因新证据持续改变；Outcome 是否拥有独立于 Agent/conversation 的持久状态；Operating Envelope 是否在运行时真正动态约束动作；Receipt 是否足以重建关键决策链。

## 跟踪判断

**值得持续跟踪：5/5。**

跟踪重点不是 Oracle 新增多少 Agent，而是 Outcome Run 是否真的成为企业软件的新一等对象，以及第三方产品是否独立收敛到同一结构。

## 主要证据

- Oracle, “Oracle Extends Fusion Agentic Applications with Introduction of Fusion Claw”, 2026-09-29.
- Oracle Docs, “Use Oracle Fusion Claw to create autonomous outcomes with agentic applications”, 26D.
- Oracle Docs, “Fusion Claw Capabilities”, 26D.
- Oracle CX Blog, “Introducing Fusion Claw”, 2026-09-29.
