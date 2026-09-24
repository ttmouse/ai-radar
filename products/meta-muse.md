# Meta Muse

**创新评分：★★★★★**  
**模式：Agent Workspace + Independent Governance Layer**

## 真实问题
长时间运行的 Agent 需要浏览器、文件、登录状态、任务进度和执行环境；如果治理逻辑直接混在执行 Agent 内部，系统既难审计，也难限制越权。

## 核心机制
给 Agent 一个持久 Workspace/VM，让它能持续工作；同时把安全与监督放在独立层，由 Sentinel/治理组件观察行为并限制高风险动作。

## 为什么不是普通加 AI
不是“更长的聊天上下文”，而是把 Agent 设计成拥有持续工作环境的数字执行者，并把治理从执行逻辑中解耦。

## Composable Test
VM + browser + model + policy engine 可以拼装，但架构上“执行与治理分离”是关键变化。

## 可迁移原则
> 不要让执行者自己决定自己是否安全；执行 Runtime 与 Governance Runtime 应分层。

## 对本地 Agent / B/G 的启发
本地 Agent 可以拥有项目目录、浏览器 session、CLI、任务状态；权限、审批、审计则由外层 Runtime 管理。应急场景尤其不能让业务 Agent 同时拥有判断与无限制写操作权。

## 2026-09-23 对话后更新：Human fallback 不是 HITL，而可能是 Runtime capability

### 已确认事实
Reuters 2026-09-22 报道：Meta 正在内部测试 Muse 的 “human concierge / human agent calls”。当 Muse 的电话任务需要处理现实世界事务时，可以把请求交给受训人工，由人工实际完成电话；之后仍由 Muse 产品向用户承接任务结果。Meta 表示该能力仍在测试，并承诺正式推出前加入适当披露。部分员工提出了隐私担忧。

### 判断变化
这不是新的一级 Pattern，暂不修改 patterns.md。它更像对 Pattern 14（Agent Workspace + Governance Layer）和 Pattern 17（Judgment Queue）的一个反向扩展：过去我们主要讨论 AI 执行、人在高风险节点审批；这里出现了另一种结构——Agent 是用户面对的长期责任主体，但底层执行资源可以动态切换为 AI、确定性软件或人工。

可以抽象为：

User Intent → Agent-owned Task → Capability Router → {AI / Software / Human} → Result → Agent-owned State

关键不是“Human in the loop”，而是 **Human as a callable capability / fallback runtime**。如果这一机制成熟，用户不需要知道任务究竟由模型、API、浏览器还是人工完成；产品对外保持一个持续的 Agent 身份与任务状态，内部按可靠性、权限、成本和可用性选择执行资源。

### Composable Test
这一体验在技术上很容易拼：Agent + call center / human ops queue + handoff + transcript + state machine，1–2 天可以做出 80% 原型。因此它本身不构成强产品机制创新，也不新增 Pattern。真正难点在运营、隐私、披露、责任归属和 handoff context 的完整性。

### 反证与风险
如果人工是为了掩盖模型能力不足，而且用户误以为始终由 AI 执行，那么它不是 AI-native 创新，而是“Wizard of Oz”式能力补洞。Reuters 报道明确提到部分员工担忧用户敏感信息可能被意外暴露给呼叫中心承包商；Meta 也表示正式发布前需要适当披露。因此这里最重要的验证指标不是任务完成率，而是：用户是否知道何时发生人工接管、授权是否覆盖人工、哪些上下文允许传给人工、责任和审计如何记录。

### 对本地 Agent / B/G 的启发
不要把 Human-in-the-loop 只设计成“审批按钮”。更完整的 Runtime 可以有三类执行资源：Deterministic Software、AI Agent、Human Operator。任务对象持续存在，执行者可以切换；但必须把 execution_actor、handoff_reason、shared_context、permission_scope、evidence、audit 显式记录。

在应急管理里，AI 无法判断某张隐患照片时，不一定只是让专家点“通过/不通过”；系统也可以把“补充核查”作为一个可委派给专家的子任务，专家完成后结果回到原任务状态，而不是另起一个割裂流程。

## 跟踪判断
**强烈值得跟踪。** 重点观察 human concierge 是否公开上线，以及 Meta 是否把人工接管的 disclosure、permission、audit 和 context boundary 产品化。