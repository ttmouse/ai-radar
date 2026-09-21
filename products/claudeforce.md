# Claudeforce / AIforce

**创新评分：★★★★☆**  
**模式：09 — Headless System + Generative Interface**

## 真实问题
传统企业 SaaS 把大量价值绑定在固定页面、菜单、字段和 Dashboard 上。用户必须先学习“软件怎样组织”，再把自己的工作意图翻译成页面操作。Agent 出现后，这层固定 UI 开始成为中间损耗。

## 旧工作流
用户意图 → 打开 Salesforce → 找对象/页面 → 阅读记录 → 跨页面拼 Context → 修改字段/触发 Workflow。

## 新工作流
用户意图 → Claude / Slack / Coworker 等工作表面 → AIforce 读取 Salesforce 的数据、语义、业务逻辑、权限和 Workflow → 动态生成当前任务所需的 live interface / insight → 动作仍回到 Salesforce 的确定性规则和权限层执行。

## 核心机制
AIforce 在 2026-09 Dreamforce 后把早期 Claudeforce 的方向进一步产品化：Salesforce 不再把固定 UI 视为唯一入口，而是明确把自己拆成两层：

1. **可信业务底座**：Data 360、Customer 360、System of Record、语义、权限、业务规则、Workflow、Actions。
2. **Live Interface Layer**：Claude、Slack、Lightning/Coworker 等任意 AI surface 可按当前意图动态组织界面和动作。

关键不是“CRM 接入 Claude”，而是 **UI 从产品预先设计的固定结构，变成 Runtime 根据 Intent + Context 临时生成的任务表面**。

## Composable Test
简单的 `LLM + Salesforce API/MCP + generative UI` 在 1–2 天内可以拼出 Demo，因此 AIforce 本身不构成新的 Pattern。

真正难复制的不是生成 UI，而是：生成出来的任意界面仍然共享同一套业务语义、权限、规则、审计和确定性动作层。也就是说，**AI 可以自由组织表达层，但不能自由发明业务事实和业务规则。**

因此它强化 Pattern 09，而不是新增模式。

## Human / AI / Deterministic Software 分工
- **Human**：表达目标、判断结果、处理高风险/例外决策。
- **AI**：跨记录理解 Context、决定当前需要展示什么、动态组织 UI、建议并发起动作。
- **Deterministic Software / Salesforce**：保存权威状态，执行权限、业务规则、Workflow、Action、审计与治理。

## 技术 / Runtime / Context 判断
### 已确认
- AIforce 被 Salesforce 定义为 live interface layer。
- Salesforce 将 Data 360 与 Customer 360 的数据、语义、权限、业务逻辑和动作暴露给不同 AI surfaces。
- Claudeforce 提供预置 Salesforce skills；动作仍通过 Salesforce 执行以保证业务规则。
- Slackforce Surfaces 可以根据需求生成可交互的 live interface。
- Headless Toolkit 通过 MCP、API、plug-in、skills 等开放底层能力。

### 合理推断
- 长期产品形态会弱化“每个角色一套固定后台页面”，强化“共享业务底座 + 按任务生成工作表面”。
- 企业 SaaS 的设计重点将从页面 IA 向 semantic/action/permission schema 上移。

### 未知
- 动态 UI 在复杂长流程中的一致性、可学习性和可审计性是否足以替代固定页面。
- 用户是否会因为每次界面变化而失去空间记忆与操作熟练度。
- 多 Agent / 多 surface 同时修改同一业务对象时，冲突治理是否足够成熟。

## 可迁移产品原理
> **固定 UI 不再是业务系统本身。业务系统应首先成为可信、可调用、可治理的状态与动作层；UI 可以按任务临时生成。**

但不能走到“所有 UI 都生成”的极端。高频、确定、需要肌肉记忆的操作仍适合固定 UI；低频、跨对象、跨系统、强 Context 的任务最适合生成式界面。

## 对本地 Agent / B/G 的启发
对应急管理后台，更值得做的不是先重画全部后台，而是先把底层对象和动作定义清楚：责任主体、任务、检查、隐患、证据、整改、验收，以及它们的权限、状态机和可执行 Action。

然后专家工作台可以围绕任务动态组织：

`专家意图 → 当前任务 Context → 生成任务表面 → 调用确定性业务动作 → 回写权威系统`

这与“AI看 / 视频看 / 现场看不应机械等同于一级页面入口”的判断一致：这些可以是 Runtime 选择的能力，而不是固定 IA。

## 可复刻路径
如果本地已有 Agent Runtime、CLI、Browser、MCP 和原系统 API，值得直接复刻的是：

1. 统一业务对象 / Action schema；
2. 权限和状态机保持 deterministic；
3. Agent 根据任务生成临时工作面板；
4. 所有写操作回到原业务底座；
5. Browser 只补 API 不完整的部分。

不值得抄的是 Salesforce 的大而全平台包装，也不应为了“AI Native”把所有稳定页面都改成生成 UI。

## 对话后更新 / 判断变化
早期判断重点是“Headless SaaS + Generative UI”。AIforce 的正式发布让判断更具体：**真正的产品边界不是 Headless，而是把 Interface 从设计期资产变成 Runtime 资产，同时把 State / Semantics / Permission / Action 固定在可信底座。**

这比“UI is AI”更准确，也避免把 Generative UI 本身误判为创新。

## 跟踪判断
**★★★★☆，继续跟踪。**

它没有新增 Pattern，但正在把 Pattern 09 从概念验证推进到大型企业软件的正式架构方向。最值得观察的不是 Claude 集成数量，而是企业用户是否真的开始减少对固定 Salesforce UI 的依赖。