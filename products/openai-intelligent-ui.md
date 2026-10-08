# ChatGPT Intelligent UI｜长期产品机制档案

> 首次研究：2026-10-08；功能发布日期：2026-10-07；状态：Core（已有模式的强扩展）；Tracking 5/5；Mechanism Novelty 2/4；Evidence B；Pattern：#09（主）、#13（辅）；新 Pattern：否。

## 产品与来源

- [OpenAI 官方发布：GPT-6 and Intelligent UI for everyone](https://openai.com/index/gpt-6-for-everyone/)（2026-10-07）
- [OpenAI 开发者社区公告](https://community.openai.com/t/gpt-6-and-intelligent-ui-in-chatgpt/1404139)（2026-10-07）
- [TechCrunch 发布现场演示报道](https://techcrunch.com/2026/10/07/chatgpt-is-getting-a-lot-more-visual-with-the-launch-of-a-new-interface/)（2026-10-07）
- [Anthropic 官方先例：Visual and interactive content](https://support.claude.com/en/articles/13641943-visual-and-interactive-content)（2026-03-16）
- 研究对象是 **ChatGPT 的 Intelligent UI 应用层机制**，不是 GPT-6 模型能力更新，也不是 OpenAI dots（独立档案：openai-dots.md）。

## 真实问题

固定聊天消息格式将不同任务都变成文本输出。用户需要比较、选择、计算、调参或理解空间结构时，只能阅读文本后继续打字或转到其他工具。反复追问的成本与认知负担往往高于交互本身。

## 旧工作流 / 新工作流

**Before**：意图 → 文字说明/表格 → 人阅读并脑内模拟 → 追问或复制到其他应用 → 再调整 → 返回聊天。

**After**：意图 → 模型选择最小合适的表达与交互结构 → 原生组件与内容流式呈现 → 人在回答内部操作 → 界面局部更新/继续对话。

**具体例子**：问「五人聚餐要买多少食材」不只是返回定量列表，而是显示人数控制与动态数量；问「自行车结构」不只是文字介绍，而是可选部件的交互图；问「储蓄增长」则给可调计算器。

## 核心机制与交互原语

**Answer → Intent-shaped Interactive Surface**。输出从文本 token 变为组合了视觉、控件、局部交互状态的原生响应。重要的是 **不要求用户显式发起“创建应用”任务**，模型在普通问答中判断何时交互比文本更有效。

三层分工：
1. AI 判断当前意图需要什么交互表达，以及信息如何布局。
2. Deterministic renderer/compiler 将组件结构逐步编译、渲染、维护交互与确定性计算。
3. Human 通过点选、调参、探索和追问参与，不再只能通过文字反复解释。

**区别于“生成 HTML 页面”**：界面是主回答格式的一部分，渐进生成、受统一组件体系约束，不是每次输出独立可执行站点。

## Composable Test（严格）

**1–2 天可复刻**：LLM → JSON schema → React/Vue 组件 → 图表/表单 → 前端本地状态，做出比较卡、计算器、证据卡和选择器。Demo/Happy Path 易复制。

**仍保留结构性差异**：把“表达形态的选择”内化到默认问答工作流；开放域问题自动选 UI 或纯文本；流式增量渲染；统一视觉规范；面向大量日常任务的一致性与可用性。单个 Demo 的可拼装性不意味着「默认交互方式」已经被复制。

**判定**：Composable but Structural；**不是全新机制**，因为 Claude 早在 2026-03 已有官方文档描述可自动生成交互视觉和结构化输入，且 Radar #09 已涵盖 Generative Interface。

## Default Workflow Test

- **主路径**：官方明确适用于 ChatGPT Chat 的日常提问，而非单独开发者工具；强信号。
- **旧步骤是否减少**：部分问题可免去复制到表格/创建独立工具；尚无定量证据。
- **持久状态**：尚无证据证明生成 UI 变成可长期维护的业务对象。
- **实际采用**：全球滚动发布是可用性信号，不等于用户长期留存或任务成功率。
- **初步判断**：有机会成为默认交互层，但必须验证是否真正减少认知负担与操作成本。

## Human / AI / Deterministic Software 分工

| 角色 | Before | After |
|---|---|---|
| Human | 读解释、想象、继续打字 | 直接操作结构化结果、选择、调参 |
| AI | 生成说明/代码 | 选择表达方式、组织组件和内容 |
| Software | 固定聊天 UI / 文本渲染 | 受约束组件、编译、局部状态、确定性计算 |

重要边界：**生成按钮不是获得权限**。真实写操作必须依赖原业务系统权限、审计和必要审批。

## 技术架构 / Runtime / Context / Memory / Tool / Approval / Eval

### 已确认事实

- 官方披露 native, streamable components library。
- 官方披露 compiler 在模型生成时处理 UI，使内容逐步出现。
- 官方披露针对内容、布局、视觉、交互决策的训练和评估，评价目标包含清晰、实用、完整。
- 官方 2026-10-07 开始面向付费层发布，次日扩展 Free/Go；当前作用于 Chat，不改变 Work/Codex 模型。

### 合理推断（非官方内部实现）

- 很可能存在受约束的 UI 结构化表示与组件注册表、增量解析/渲染、客户端局部交互状态。
- 可计算部分应尽量用确定性执行，而不是每次点击都重新让模型计算。
- UI 生成与 Tool 授权应该是分离的安全边界。

### 未知与不得虚构

UI DSL/schema、编译器实现、服务端/客户端职责、组件白名单、状态持久化、跨会话可恢复、外部业务动作授权、完整 Eval 结果、错误回滚、用户真实任务完成率、成本/延迟。没有证据说明其自带独立 Agent Runtime、Memory 或审批链。

## 可迁移产品原理

**Interface is a function of intent, not a fixed destination.**

选择表达方式本身是 AI 可以承担的新决策，但稳定的业务语义、权限、数据与状态不应因此消失。高质量生成式 UI 的成功标准是「用户更少努力、更少错误地完成目标」，不是「生成更多控件」。

## 对 AI 产品设计 / AI 原生研发 / 本地 Agent 的启发

- **设计**：给每类认知任务配置合适原语：比较 → 矩阵；解释 → 可探索图；参数调整 → 控件；证据 → 来源卡；正式动作 → 审批对象。
- **研发**：先建 5–8 个可测、可访问、可约束的原生组件，而不是任意 HTML；模型只输出 schema，渲染器做校验。
- **本地 Agent**：在已有 Runtime/CLI/Browser/MCP/File 基础上补 **result-to-interaction layer**；Agent 的结果可以变成可确认、可修正、可继续执行的对象。
- **应急/B/G**：专家查看风险可动态切换证据比较、风险趋势、整改差异、待判断队列；正式隐患对象、检查单、三联单、督办、验收和签发仍以稳定业务模型和授权链为准。生成 UI 是视图，不是 Source of Truth。

## 本地复刻路线与不值得抄

**建议 MVP（1–2 天）**：
1. 选三个高频任务：两家责任主体风险比较、隐患证据审查、整改期限调整。
2. 定义固定组件：CompareCard / EvidencePanel / ParameterControl / ProposedAction / Timeline。
3. Agent 返回受限 schema + evidence_refs + permitted_actions；Renderer 负责组件实例和确定性计算。
4. 任何正式写操作都只生成 Proposed Action，由 RBAC/Policy/审批链执行。
5. A/B 测试：纯文本 vs 动态 UI 的完成时间、误判率、追问次数、审批撤回率。

**不值得抄**：从零实现通用流式编译器；任意执行模型生成脚本；无上限扩展组件库；把所有专业界面都生成化；用 UI 美观代替事实正确性。

## 最大未知、反证与高估风险

- **原创性反证**：Anthropic Claude 2026-03-16 已有同类机制；不可声称 OpenAI 首创。
- **正确性反证**：更漂亮的交互可能让用户更相信错误数据。
- **产品性反证**：微工具若不可持久、共享、审计，仍只是一次性答案，不是新 System of Record。
- **性能反证**：组件加载、流式编译、移动端触控可能引入新摩擦。
- **用户行为反证**：如果大多数用户关闭交互或回到文本，默认工作流假设不成立。
- **Agent 反证**：这不证明更强 Agent 权限、持续责任或业务执行能力。

## 与历史模式的 nearest-neighbor

- **#09 Headless System + Generative Interface**：最接近。此次强化了对话响应层的默认生成式 UI，未推翻 #09。
- **#13 Deterministic / Probabilistic Split**：模型负责选择表达，软件负责渲染/计算/权限。
- **#10 Artifact → Semantic Object → Action**：只有连接真实业务动作时才算。
- **#15 Proposed Action**：若可操作组件触发高风险动作才需要此对象。
- **#28 Generation as State Transition**：没有证据表明生成界面成为长期工作状态。

**本次新增的是强规模与默认工作流证据，不是新 Pattern。**

## 跟踪判断与待验证问题

**Tracking 5/5，Novelty 2/4，Evidence B。** 下一次优先调查：
- 生成 UI 的输入/状态是否能保存、复用、分享？
- 交互结果是否进入模型 Context、是否可重演？
- 组件与外部工具的安全边界在哪里？
- 是否能提供对真实任务的任务完成率/错误率评测？
- 在企业业务场景是否会从临时答案变成稳定工作视图？

## 对话后更新 / 判断变化

（后续用户提出新的实质性判断、反驳、落地路径时，在本文件继续追加，保留证据与日期，不创建重复档案。）
