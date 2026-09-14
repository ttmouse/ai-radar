# Frigade Assist API

**创新评分：★★★★★**  
**状态：Core**  
**模式：Product → Learned Operational Model → Guidance / Action**  
**首次记录：2026-09-14**

## 产品入口

- 官网：https://frigade.com/
- Assist API：https://frigade.com/assist-api
- How it works：https://frigade.com/how-it-works
- 发布说明（2026-09-08）：https://frigade.com/updates/assist-api
- 创始团队解释：https://frigade.com/blog/your-product-cant-explain-itself
- Self-updating product knowledge：https://frigade.com/features/always-accurate
- AI-generated tours：https://frigade.com/features/ai-generated-tours
- Skills（自动发现动作）：https://frigade.com/updates/skills

## 1. 一句话判断

Frigade Assist API 最值得看的不是“给现有 Agent 加一个产品帮助工具”，而是它把**软件本身**从被动 UI 变成了 AI 可以持续学习的 Source of Truth：浏览器 Agent 像真实用户一样运行产品，形成一个持续更新的操作模型；现有 Agent 再用这个模型回答、在界面里带用户完成流程，甚至执行动作。

核心变化：**产品知识不再主要由人写文档，而由 AI 从软件实际行为中获得。**

## 2. 它解决的真实问题

今天绝大多数 in-product Agent 的产品知识来自：

- Help Center
- Docs
- FAQ
- Release Notes
- 人工维护的 Prompt / KB

问题是产品持续变化，而文档天然滞后。按钮改位置、流程换名字、权限变化、新功能上线以后，Agent 仍可能按照旧文档回答。

Frigade 把问题重新定义为：

> 为什么让 Agent 通过“别人对软件的描述”认识产品，而不是直接让 Agent 自己使用产品？

## 3. 过去的工作流

```text
产品上线 / 改版
→ PM / Support / PMM 发现帮助内容需要更新
→ 人写 Help Center / FAQ / Tour
→ 工程埋点或绑定 selector
→ 用户提问
→ Chatbot 检索文档并回答
→ UI 再次变化
→ 文档 / Tour 再次失效
```

问题并不是 LLM 不够强，而是 **Context Source 本身过时**。

## 4. 新工作流

Frigade 官方描述的机制更接近：

```text
给 Frigade 一个真实用户身份 / staging 访问权限
→ Browser Agent 实际操作产品
→ 遍历真实 workflow
→ 记录功能、页面、步骤以及它们之间的关系
→ 与现有 Docs / KB 合并
→ 形成持续更新的 Product Model
→ 现有 Agent 通过一次 tool call 查询这个模型
→ 返回：Grounded Answer / Live Walkthrough / Action
→ 每次产品发布后自动重新学习变化
```

用户侧则是：

```text
“怎么添加 webhook？”
→ Agent 不再丢一篇帮助文档
→ 识别用户当前页面和权限
→ 打开正确页面
→ 高亮实际按钮
→ 一步一步指导
→ 必要时直接代用户完成动作
```

## 5. 最关键的产品机制

### 5.1 Product as Source of Truth

最重要的一点不是 RAG，而是 **Grounding Source 变了**。

传统：

```text
Product → Human Documentation → Knowledge Base → Agent
```

Frigade：

```text
Product → Browser Agent Exploration → Operational Product Model → Agent
```

文档退为辅助信源，软件实际行为成为主要信源。

### 5.2 Operational Product Model

官方直接使用 “Product model” 描述底层能力：它不是只存页面文字，而是记录 workflow 怎样连接、每个角色能看到什么、动作怎样执行。

这里重要的是“程序性知识（procedural knowledge）”：

> 系统不仅知道“Webhook 是什么”，还知道“在当前版本、当前权限下，如何真正创建一个 Webhook”。

### 5.3 Self-relearning

Frigade 会在产品发布后重新学习，按钮移动或流程调整后，操作模型随产品更新。

这使产品知识从静态 Artifact 变成一个**持续维护的状态**。

### 5.4 Learn → Teach → Act

Frigade 自己先作为 Agent 学习产品，然后把学到的能力暴露给另一个用户 Agent。

结构上是：

```text
Learning Agent
→ Operational Model
→ User-facing Agent
→ Human
```

这是比“一个 Agent 直接操作网页”更值得看的地方。

## 6. 为什么不是普通“加 AI”

如果 Frigade 只是：

```text
Docs + RAG + Chatbot
```

完全不值得进入核心名单。

如果只是：

```text
LLM + Browser Use
```

同样可以在 1–2 天用现有 Browser Agent 拼出 Demo。

它真正通过 Composable Test 的部分是把下面这一整条变成产品闭环：

```text
Autonomous Product Exploration
→ Workflow Mapping
→ Role-aware Product Model
→ Release Change Detection
→ Automatic Re-learning
→ Runtime Guidance / Action
→ Human Steering
```

单个组件都可拼，**持续维护一个“软件如何实际工作”的 Operational Model** 才是产品机制。

## 7. Human / AI / Deterministic Software 分工

### Human
- 给学习 Agent 正确权限
- 决定哪些动作可以开放
- 修正错误回答
- 定义高风险动作的边界

### Learning AI
- 像真实用户一样遍历软件
- 发现 workflow
- 建立 / 更新 Product Model
- 检测版本变化

### User-facing AI
- 理解用户意图
- 判断是否调用 Frigade
- 基于 Product Model 回答 / 指导 / 执行

### Deterministic Software
- 身份与权限
- 页面真实状态
- SDK / UI overlay
- 操作执行
- 日志、审计、版本

这里的原则仍然很清楚：**AI 负责理解与发现，确定性系统负责真实权限和动作落地。**

## 8. 架构判断：事实 / 推断 / 未知

### 已确认事实

官方明确披露：

- 浏览器 Agent 使用真实用户权限登录产品；
- Agent 会实际运行 workflow 并映射产品结构；
- 产品每次发布后会重新学习；
- Product Model 是 Assist API 下方的核心引擎之一；
- 可以生成基于当前 UI 的 live walkthrough；
- Skills 可以在学习产品过程中自动发现 create / edit / delete 等动作；
- Skill 需要团队批准后才开放；
- Guidance 遵守当前用户权限；
- Support / CX 可以纠正 Agent 回答而无需改代码。

### 合理推断

一个可用的 Operational Product Model 大概率至少包含：

- Page / State
- UI element semantic identity
- Workflow graph
- Preconditions
- Role / Permission
- Action
- Transition
- Expected outcome
- Failure / alternative path

官方没有公布具体 Schema，因此这是产品架构推断，不应当当成已确认实现。

### 未知

- Browser Agent 如何系统性保证 workflow coverage；
- 产品变化时是全量重跑还是增量 diff；
- 如何判断一个 workflow 已“学会”；
- Eval set 如何构建；
- 动态 SPA、大量条件分支、复杂权限情况下的覆盖率；
- 错误操作模型如何被检测和回滚。

这些决定它究竟是强产品还是好看的 Demo。

## 9. 与已有模式相比，它真正新增了什么

### vs Browzer：Source → Self-maintaining Artifact

Browzer 的核心是：Source 变化后，派生文档自动保持一致。

Frigade 更进一步：

```text
Software itself
→ AI learns operational behavior
→ produces a living executable model
→ drives real user guidance/action
```

所以不是“文档自动更新”，而是**软件自动产生自己的程序性知识层**。

### vs Airtop / Nex

Airtop/Nex 讨论的是 AI 与确定性执行之间的分工。

Frigade 的新增是：**AI 如何获得“这个具体软件怎样工作”的知识。**

### vs Tadata

Tadata 观察真实用户工作，寻找“哪些行为值得自动化”。

Frigade 则让 synthetic browser agent 主动探索软件，学习“这个产品具备哪些 workflow”。

一个是在发现人的流程，一个是在建立软件的操作模型。

## 10. 可迁移产品原理

> **当软件本身已经是最准确的知识源时，不要再让人手工把软件翻译成 AI Context；让 AI 直接从运行中的软件建立可执行知识。**

这可能是一个比“RAG over docs”更长期的方向。

以后很多 AI Context 不一定来自 Markdown / 文档，而可能来自：

- 实际 UI
- API 行为
- 系统状态
- 权限
- 流程执行轨迹

## 11. 对你的本地 Agent / 应急专家工作台的启发

这条对你非常直接。

你现在面对的现实是：原政府后台不会消失，而你希望用浏览器插件 / 本地 Agent 渐进式重构工作流。

过去通常有两条路：

### 路线 A：人工适配每个页面

```text
研发分析旧系统
→ 写 selector / API adapter
→ 写 workflow
→ 后台改版
→ 再维护
```

### 路线 B：让通用 Browser Agent 每次重新理解

```text
用户下任务
→ Agent 每次临时看页面
→ 推理下一步
→ 慢、贵、不稳定
```

Frigade 给出第三种可能：

```text
Learning Agent 预先探索良渚后台
→ 建立“企业、任务、隐患、检查单、督办”等操作模型
→ 产品运行时 Agent 直接消费这个模型
→ 普通流程快速执行
→ 页面变化时重新学习
```

这正好处在“纯 API 重构”和“每次都让 Browser Agent 现场推理”之间。

## 12. 本地可直接复刻什么

你已经有：

- 本地 Agent Runtime
- Browser / WebView
- Cookie / 登录状态
- CLI
- MCP
- 原系统 API
- 本地文件访问

因此 V0 可以非常具体：

### V0：只做一个业务流程

选择一个稳定流程，例如：

> 找到某企业 → 打开隐患 → 查看整改证据 → 进入验收入口

让一个 Learning Agent 在测试账号里完成多次，并保存：

```text
Goal
Current page/state
Action
Target semantic element
Resulting state
Required role
Evidence
```

然后形成一个 workflow graph。

运行时，专家只说：

> “打开这家企业最新待验收隐患。”

Agent 优先按照已学 workflow 执行；只有路径失效时再进入 Browser reasoning。

### V1：Change Detection

每天或版本变更后跑关键 workflow：

- selector / semantic element 是否还存在
- 页面是否移动
- workflow 是否仍能完成

失败时才让 AI 重新学习并产生 diff。

### V2：Guided UI

不一定让 Agent 直接替专家点击。

对于高风险操作，可以像 Frigade 一样：

```text
Agent 找到正确页面
→ 高亮需要操作的位置
→ 解释为什么
→ 人点击确认
```

这比完全自动化更适合 B/G 场景。

## 13. 哪些不值得抄

### 1. 不要先做完整产品导览系统
你的价值不是 Product Adoption SaaS。

### 2. 不要把所有后台页面都“学一遍”
先学专家高频、价值最高的 5–10 个 workflow。

### 3. 不要用视觉 Agent 替代已经稳定的 API
如果某动作已经有可靠 API，继续走 API。

Operational Model 应该记录“怎样完成任务”，而不是强迫所有任务都走 UI。

### 4. 不要相信“自动学习”就等于可靠
真正难的是 Coverage、Eval 和 Change Detection。

## 14. 最大未知 / 反证

### 可能被高估 1：复杂产品覆盖率
Demo 中几条 workflow 很漂亮；真正企业后台可能有数百状态、角色和异常分支。

### 可能被高估 2：UI 不等于业务规则
Agent 能学会“点击哪里”，不一定理解后台业务为什么允许这么做。

因此在 B/G 系统里，Operational Model 必须与 API、业务规则和权限模型结合。

### 可能被高估 3：维护成本只是被隐藏
如果每次 release 后仍需要大量人工检查学习结果，那么“self-learning”只是把维护方式换了一个位置。

## 15. 跟踪判断

**5 / 5，进入核心长期跟踪。**

值得跟踪的不是 Frigade 会不会成为最大的 Product Adoption 公司，而是它代表的机制：

> **Software can become its own continuously learned operational context for AI.**

对于你正在做的浏览器插件 / 专家工作台，这比再找一个“更强 Browser Agent”更值得研究。
