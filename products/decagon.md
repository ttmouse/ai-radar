# Decagon

## 跟踪判断

- 状态：**Core / 持续跟踪**
- Tracking Score：**5/5**
- Mechanism Novelty：**3/4**
- Evidence Maturity：**B**（官方产品发布、机制说明与协议说明较完整；跨企业长期实际运行证据仍有限）
- 本轮核心：**Personal Agent Gateway + PACT**
- 官方证据：
  - https://webflow2.decagon.ai/blog/dialogues-2026
  - https://webflow2.decagon.ai/blog/personal-agents-are-here
  - https://webflow2.decagon.ai/blog/voice-3

## 真实问题

个人 Agent 正开始代表用户去订票、购买、取消、谈判和联系客服。传统客户服务系统默认请求者就是账户本人：网页、电话、聊天渠道都围绕“人直接操作”设计。一旦请求者变成 Agent，企业必须同时回答四个问题：它是不是 Agent、它代表谁、用户到底授权了什么、企业应该按什么政策与它交互。

如果只让个人 Agent 继续模拟人点击网页或打电话，企业既无法可靠识别代理关系，也难以把授权范围与具体动作绑定；如果完全开放，又会带来权限、审计、无限重试和策略博弈问题。

## 旧工作流

```text
Human
→ Website / App / Phone / Chat
→ Business UI / IVR
→ Human or support bot
→ Enterprise system
```

软件默认“操作者 = 权利主体”。授权通常隐含在登录 Session、账户验证或人工身份核验中。

## 新工作流

```text
Human / Principal
→ delegates scoped authority
→ Personal Agent
→ agent-native channel / PACT
→ Business Agent
→ enterprise policy + AOP + deterministic permission checks
→ Enterprise system

必要时：
Personal Agent → Human owner escalation
Business Agent → Enterprise human escalation
```

关键变化不是“两个 Agent 聊天”，而是企业开始把 **代理关系（delegation）** 当成需要显式表达和验证的业务关系。

## Mechanism Delta

**Before：** 企业系统主要识别“哪个人/账户正在操作”。

**After：** 企业系统还必须识别“哪个 Agent 正代表哪个 Principal，在什么 Scope 下行动”。

因此 Identity 不再足够，系统需要：

```text
Principal
  ↓ delegates
Agent Identity
  ↓ carries
Authorization Scope
  ↓ constrained by
Business Policy
  ↓ produces
Action + Audit Evidence
```

这使 **Delegated Authority** 从隐含 Session/登录关系，升级为 Agent 时代的一等运行时关系。

## 最关键的产品机制

### 1. Personal Agent 变成新的客户类型

Decagon 不要求个人 Agent 继续伪装成人类使用传统 UI，而是提供独立 agent channel。企业可以为 Agent 和真人配置不同 AOP / workflow / policy。

### 2. 权利主体与执行主体分离

用户仍然是 Principal，但实际发起动作的是 Personal Agent。PACT 用 delegated authorization 表达“它代表谁、被允许做什么”。

### 3. 双边治理

个人 Agent 有自己的目标；Business Agent 代表企业政策。两边不是共享一个目标函数。系统必须允许协商、拒绝、升级，而不是假定 Agent-to-Agent 自动合作就会收敛。

### 4. 权限进入工作流本身

权限不是外围 IAM 设置。AOP 的具体区段可以要求特定 scope；没有 scope 时对应流程不向 Agent 暴露。授权因此进入业务流程语义。

## Composable Test v2

### 可以快速复刻的部分

使用 OAuth 2.0 + A2A/MCP + Agent Runtime + policy engine + approval UI，可以在 1–2 天做出：

- Agent 身份声明；
- 用户授权页面；
- scope token；
- 两个 Agent 交换请求；
- 高风险动作二次审批；
- 基础审计日志。

### 为什么仍然通过

快速组合能复制协议 Happy Path，却不能消除这里的产品结构变化：**过去企业软件默认 Human 是直接 Actor；现在 Principal、Delegate、Business Counterparty 被拆成不同一等角色。**

真正的新东西不是 OAuth，也不是 A2A，而是 Agent 成为客户后，企业服务边界从 Human-facing Interface 扩展为 **Delegation-aware Service Boundary**。

结论：**Composable but Structural**。

## Human / AI / Deterministic Software 分工

| 角色 | 过去 | 新结构 |
|---|---|---|
| Human | 自己发起并操作 | 定义目标、授予 scope、处理例外 |
| Personal Agent | 不存在/辅助 | 代表 Principal 持续提出和推进请求 |
| Business Agent | FAQ/客服自动化 | 代表企业解释请求、执行政策、协商与升级 |
| Deterministic Software | 登录、ACL、交易规则 | 验证 delegation、scope、policy、audit，守住不可由模型决定的边界 |

## 技术 / Runtime / Context：事实与推断

### 已确认事实

- Personal Agent Gateway 会识别 personal agent，并提供专用 agent channel。
- 同一请求可因请求者是 Human 还是 Agent 而走不同 AOP。
- 企业定义 scopes，用户明确授予；缺少 scope 时需要权限的 AOP 部分不会暴露。
- PACT 构建在 Agent2Agent 与 OAuth 2.0 delegated authorization 之上，用来证明 Agent 代表哪个人以及被授予什么权限。
- 两侧都存在 escalation：Business Agent 可升级给企业员工，Personal Agent 可回到 owner。

### 合理推断

- 长期会需要可撤销、可过期、目的限制、动作级证据以及跨 Agent Provider 的 delegation records。
- Agent-to-Agent 请求频率远高于 Human 请求后，rate limit、经济成本与反滥用会成为服务政策的一部分。
- 企业 UI 会逐步从唯一产品表面退化为 Human channel 之一，Agent channel 成为并列的一等入口。

### 未知

- PACT 是否会获得 Decagon 之外的广泛采用。
- personal agent detection 的准确率与绕过成本。
- scope 粒度是否足以表达真实世界复杂委托，例如金额、时间、目的、次数和条件限制。
- 两个 Agent 长时间协商时如何证明没有越权或策略性诱导。

## 与现有 Pattern 比较：真正新增了什么

它明显强化：

- **#14 Agent Workspace + Governance Layer**：Agent 行为必须被外部治理。
- **#15 Proposed Action as First-class Object**：高风险动作需要显式授权/审批。
- **#22 Agent as First-class Identity**：Agent 需要独立 identity、scope、audit。
- **#23 Adversarial Agents + Independent Referee**：双方 Agent 的利益并不天然一致。
- **#26 Agent-facing System of Record**：企业系统开始提供 Agent 原生入口。

但这些模式都没有完整表达一个关键关系：**Agent Identity ≠ Principal Authority。**

今天先记录机制候选：

> **Delegated Agency / Principal → Agent → Scoped Authority**

暂不新增 #31。需要等待至少另外两个独立产品/协议证明“delegation 本身成为稳定业务对象”，而不是 Decagon 的局部实现。

## 可迁移产品原理

1. **Identity 与 Authority 分离。** 知道“哪个 Agent”不等于知道“它有权代表谁做什么”。
2. **Agent 是新的渠道，也是新的 Actor Type。** 不应该永远让 Agent 模拟人类 UI。
3. **授权应进入业务语义，而不是只停留在基础设施 IAM。**
4. **Agent-to-Agent 不等于合作。** 双方有不同目标、政策与升级路径。
5. **Human-in-the-loop 从“每一步操作”迁移到“授权边界与例外”。**

## 对本地 Agent / AI 产品设计的启发

如果本地 Agent Runtime 已有 CLI、Browser、MCP、本地文件访问，下一步不应该只增加更多 Tool，而应该建立统一 delegation object：

```text
Delegation
├── principal
├── agent_id
├── scope
├── resource
├── constraints
├── reason / purpose
├── issued_at
├── expires_at
├── revocation
└── audit trail
```

Tool execution 前由确定性 Runtime 校验 Delegation，而不是让 LLM 自己判断“用户应该允许我做”。

## 对应急管理 / B/G 产品的启发

这与政府/专家系统尤其相关。未来 AI 专家代理可能代表专家、企业安全员或街道工作人员执行部分工作，但“Agent 属于谁”和“它此刻被授权做什么”必须分离。

例如：

```text
专家
→ 授权 Agent 查看某批企业检查资料
→ Agent 可以分析/生成检查建议
→ 但不能自动签发执法文书
→ 需要新的 scope / 人工确认
```

因此权限模型不能只有 RBAC：

`User Role → Permission`

未来还需要：

`Principal → Delegation → Agent → Scoped Action → Audit`

## 可以直接复刻

- Delegation 数据模型；
- scope-based Tool Gateway；
- 高风险动作升级；
- agent-native API/channel；
- Principal/Agent 分离审计；
- 每个动作记录“谁授权、哪个 Agent 执行、使用什么 scope”。

## 不值得抄

- 为了“Agent 感”强行做 Agent-to-Agent 聊天 UI；
- 在没有真实外部 Agent 请求量之前建设复杂协议栈；
- 把 OAuth 包装成产品创新；
- 让 LLM 自己承担最终权限判断。

## 最大未知与反证

最大的反证是：PACT 可能只是 OAuth delegated authorization 在 Agent 场景中的重新包装。如果未来 personal agent 仍主要通过 Browser/UI 模拟人类，企业没有动力建设 agent-native channel，那么 Decagon 的结构判断会被高估。

真正需要观察的是：**企业是否开始把“代理关系”存成长期可查询、可撤销、可审计的业务对象，并围绕它改变服务工作流。**

## 跟踪判断

**5/5，继续重点跟踪。**

不是因为 Decagon 做了更多 Agent 功能，而是它把一个此前通常被软件隐含掉的关系显式化：**用户、代理执行者、企业服务方不再是同一个 Actor。** 这是 Agent 真正进入经济活动后必须解决的产品结构问题。
