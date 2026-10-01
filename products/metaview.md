# Metaview / fillmore

## 当前判断

- Status: Observe
- Tracking Score: 4/5
- Mechanism Novelty: 2/4
- Evidence Maturity: B
- First tracked: 2026-10-01
- Core hypothesis: **Criteria as First-class Operational Object**，但目前证据不足以新增 Pattern。

## 真实问题

招聘 AI 的问题已经不只是单个任务效率。intake、sourcing、application review、screening、interview、outreach、scheduling 和 ATS 更新分散后，招聘人员仍然必须人工维护跨步骤上下文，并承担 workflow orchestration。

Metaview 的目标是把招聘从“AI 帮人完成单点任务”推进到“人定义招聘标准与关键判断，Agent 持续执行整个流程”。

## 旧工作流

Human 定义职位 → 人工/多个工具搜索 → 人审核 → AI 辅助生成 → 人触发 outreach/follow-up → 人协调 screening/scheduling → 人同步 ATS → 下一轮重新拼上下文。

## 新工作流

Human 通过 JD / intake call 给出目标 → 系统形成 Ideal Candidate Profile（ICP）→ 多个 agents 共享 role/company/interview context → sourcing/review/outreach/follow-up/screening 在后台持续推进 → hiring decisions / rejections / interview results 回流并校准 ICP → Human 主要处理 shortlist、关系、override 和最终判断。

## Mechanism Delta

```text
Recruiter owns workflow orchestration + implicit criteria
→ criteria becomes persistent shared operational context
→ Agents own persistent execution; humans own calibration + judgment
```

最值得研究的不是 autonomous sourcing，而是 **招聘标准是否正在从人的隐性认知变成一个持续学习、可解释、可编辑、可被多个 Agent 消费的 Operational Criteria Object。**

## Composable Test v2

### 可快速复刻

使用 LLM + sourcing/search API + email + calendar + ATS API + workflow engine，可以在 1–2 天复刻：
- sourcing
- candidate research
- personalized outreach
- follow-up
- scheduling
- basic application review

因此“autonomous recruiting coworker”本身不通过强创新测试。

### 仍有结构差异

稳定体验还需要：
- shared role context
- persistent ICP
- feedback → criteria calibration
- candidate-level rationale
- human override
- cross-step state
- ATS synchronization
- guardrails / audit

结论：**Composable but Structural**。

## Responsibility Shift

| Role | Before | After |
|---|---|---|
| Human | 定标准、搜索、推进、跟进、同步、判断 | 定目标、校准标准、处理关系/例外、最终判断 |
| AI | 单点生成/总结 | 持续 sourcing/review/outreach/follow-up/screening |
| Deterministic Software | ATS/CRM 保存状态 | 状态、同步、权限、审计、确定性动作边界 |

Human 从 workflow operator 向 **criteria owner + judgment handler** 移动。

## Context / Memory

### Confirmed

官方称：
- 多个 Agent 共享 context；
- ICP 从 job description 开始，并根据 hiring decisions、application review outcomes、interview results 等持续改善；
- team / role preferences、resume、rubric 等可以进入 context；
- ICP 可被 review/edit/override；
- candidate recommendation 有 rationale；
- company knowledge/rules 可约束 Agent，并提供 non-compliance alerts 和 audit trail。

### Inferred

ICP 很可能已经承担“招聘意图 + 评估标准”的共享语义层作用，而不只是静态 prompt。

### Unknown

- ICP schema / versioning
- 不同 hiring manager 标准冲突如何合并
- feedback update 是 LLM synthesis、规则还是统计学习
- approval threshold / eval / failure recovery
- 是否有独立客户证据证明 end-to-end autonomous workflow 已成为默认使用方式

## 与已有 Pattern 比较

- #03 Goal → Autonomous Work：目标后持续执行。
- #04 Organization Context Layer：共享组织上下文。
- #17 Judgment Queue：Human 从执行转向判断。
- #19 Organization Learning Loop：日常决策反哺共享 Memory。
- #25 Business Semantic Layer：多个 Agent 共享业务语义。

目前 Metaview 可以被这些 Pattern 的组合解释，因此不新增模式。

### Candidate seed: Criteria as First-class Operational Object

若未来出现更多独立证据，可以考虑独立机制：

```text
Human tacit judgment
→ persistent / editable / explainable criteria object
→ multiple agents execute against it
→ human decisions continuously recalibrate it
```

晋升条件：至少三个独立产品出现同样结构，并证明 criteria 不是 prompt/config，而是具有持久状态、schema、反馈更新和跨 Agent 执行语义的一等对象。

## 可迁移原则

1. 不要只保存 conversation memory；保存工作的判断标准。
2. 人工 accept/reject 不只是结束一次任务，也应该成为 criteria feedback。
3. Agent 共享的核心 Context 应包含“怎样算好”，而不仅是“发生过什么”。
4. Human-in-the-loop 最有价值的位置可能是校准标准，而不是逐动作审批。

## 对本地 Agent 的启发

可直接复刻：
- Task/Role Criteria Object
- accept/reject → criteria feedback loop
- shared context
- rationale + override
- judgment/exception queue

不值得抄：
- 为每一步机械拆 Agent
- 把 multi-agent 数量当产品创新
- 单纯自动邮件、跟进、预约

## 对 B/G / 应急管理的启发

专家检查不应该只留下检查结果。更有价值的是逐步形成“这个任务/主体怎样判断风险”的可解释 criteria，并让专家每次接受、驳回、修正 AI 结论时校准 criteria。

潜在结构：

```text
检查目标
→ Criteria Object
→ AI 收集证据并判断
→ 专家处理边界案例
→ 专家修正反哺 Criteria
→ 下一轮检查
```

这比简单积累聊天 Memory 更接近长期专家能力沉淀。

## 最大未知 / 反证

1. 当前 strongest evidence 主要来自官方材料。
2. 若客户实际只使用 notetaker / sourcing / review 等独立模块，所谓端到端 agentic recruiting 可能仍是平台叙事。
3. ICP 持续学习可能只是 #19 + #25 的垂直实现，而不是新的一等对象。

## 跟踪重点

后续不重点跟踪“又增加了哪个 Agent”，而重点验证：

> **ICP 是否真正成为招聘工作的 Source of Truth，并逐渐取代 recruiter 脑内隐性标准。**

## Sources

- https://www.metaview.ai/
- https://www.metaview.ai/about
- https://www.metaview.ai/resources/blog/metaview-raised-an-additional-60m-to-lead-the-shift-to-agentic-recruiting
- https://www.metaview.ai/resources/blog/autonomous-recruiting
- https://www.metaview.ai/resources/blog/ai-recruiting-agents

## Update Log

### 2026-10-01

首次建立档案。结论：Observe 4/5；不因“autonomous recruiting”本身进入 Core；新增值得验证的 Candidate seed：**Criteria as First-class Operational Object**。
