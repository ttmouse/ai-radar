# OpenAI dots

## 跟踪判断

- Tracking Score: **5/5**
- Mechanism Novelty: **3/4**
- Evidence Maturity: **B**
- Status: **Core / Continue tracking**
- 首次进入 Core: 2026-10-06（产品 2026-09-29 发布；本次因 Help Center / safety docs 在发布后继续补全而从 Maturation Window 进入）

## 真实问题

传统 AI 助手的基本单位仍是 conversation / task：用户想起一件事，打开 AI，描述任务，等待结果。即使 Agent 能长时间运行，责任仍然主要由人维持：人记得目标、人重新发起、人决定什么时候该继续。

dots 试图解决的是另一层问题：**能否把一段持续责任交给一个长期存在的 AI worker，而不是不断把离散任务交给聊天机器人？**

## 旧工作流

```text
Human owns responsibility
→ notices something needs doing
→ opens AI / app
→ gives task
→ agent executes
→ returns result
→ session/task ends
→ Human remembers what should happen next
```

## 新工作流

```text
Human defines goal + operating boundaries
→ persistent dot owns ongoing responsibility
→ maintains memory / tasks / connected-app context
→ proactively continues work between conversations
→ deterministic permission + safety layers gate actions
→ Human handles consequential approvals / exceptions / judgment
→ feedback changes future behavior
```

关键不是“后台跑 Agent”，而是**责任从 session/task 跨越到一个持久 actor**。

## Mechanism Delta

**Task delegation → Responsibility delegation**

过去的软件把任务作为一次性执行单元；dots 开始把“持续替我负责某件事”变成默认产品关系。OpenAI 官方将 dots 描述为 always-on agents，可承担 ongoing responsibilities、跨对话继续工作，并在用户设定的权限边界内主动推进。

### 可能出现的新业务对象

```text
Responsibility
├── owner / principal
├── persistent agent identity
├── goal
├── memory / context
├── connected apps
├── ongoing tasks
├── custom rules
├── approval policy
├── safety policy
├── activity history
└── pause / reset lifecycle
```

目前 OpenAI 未公开证明内部真的存在名为 Responsibility 的统一对象；这是产品结构推断，不是已确认实现。

## Composable Test

### 可以在 1–2 天拼出的部分

- persistent agent loop
- scheduled/background tasks
- MCP / plugins / browser / cloud computer
- memory store
- Slack/SMS/Teams channel
- approval UI
- basic policy rules

因此单纯“always-on agent”本身不够创新。

### 为什么仍是 Composable but Structural

难复制的不是 runtime，而是把 **identity + memory + proactive work + permissions + approval + lifecycle** 收敛为用户可以长期委托责任的稳定产品关系。

如果复制品仍要求用户不断重新发 prompt、重新解释 context、重新确认目标，它只复制了执行能力，没有复制 dots 的核心产品机制。

## Default Workflow Test

强信号：

1. dot 在 conversation 之外继续存在；
2. reset 会同时删除 conversations、saved memories、scheduled tasks，说明它有独立生命周期；
3. 用户可 pause / resume，而不是只能结束一次 run；
4. Custom Rules 定义哪些动作可独立执行、预授权、执行前询问或交还用户；
5. Auto-review 在动作执行前独立检查 instruction / Custom Rules / safety requirements；
6. proactive research 有独立工具限制，说明主动工作不是普通聊天的简单循环。

未知：用户是否真的会长期把“责任”交给 dot，而不是把它当更强的通用 Agent。这个行为层证据仍不足。

## Human / AI / Deterministic Software 分工

| Actor | Before | dots |
|---|---|---|
| Human | 记住责任、发起任务、维持连续性、执行关键动作 | 定义目标/边界，处理高后果审批、例外和判断 |
| AI | 回答或执行一次任务 | 长期维护 context，主动决定 next step，跨会话持续推进 |
| Deterministic Software | session、账号、tool permission | plugin access、Custom Rules、approval state、Auto-review、core safety、pause/reset lifecycle |

最重要的变化：**Human 从 Process/Task Initiator 向 Responsibility Owner + Boundary Setter 移动。**

## 架构 / Runtime / Context / Approval / Eval

### 已确认事实

- dots 基于 GPT-6 Astra。
- 每个 dot 有自己的 cloud computer。
- 可连接 4,000+ apps（OpenAI plugin ecosystem）。
- 可通过 ChatGPT、短信、Slack、Teams 等渠道持续交互。
- 会随时间从 feedback 学习，并保持 context 支撑 ongoing work。
- Custom Rules 可将支持的动作设为独立执行、预授权、执行前确认或交还用户。
- 某些 core safety requirements 不能被 Custom Rules 或用户批准覆盖。
- Auto-review 会在部分动作前检查 instruction、Custom Rules 与 safety requirements。
- proactive research 有更严格的工具限制，不能直接发消息、通过 plugin 改内容或控制浏览器/电脑。
- pause/resume 与 reset 构成 dot 的独立 lifecycle；reset 会删除 conversations、saved memories、scheduled tasks。

### 合理推断

- dot 需要持久 state store，将 identity / memory / task / permission / activity 关联到同一 actor。
- proactive loop 与 interactive conversation 很可能使用不同 execution policy。
- Auto-review 很可能是与主执行模型分离的 policy/evaluation path，而不是只靠同一次 LLM reasoning。

### 未知

- memory 的具体 schema、写入/遗忘策略与冲突处理。
- proactive next-step 的触发机制与调度模型。
- Auto-review 的模型/规则组合、误报率和 eval 指标。
- 长周期责任失败后如何恢复、归因和重新规划。
- 多 dots 未来如何共享/隔离 context、authority 与责任。

## 与已有 Pattern 比较

最接近：

- #03 Goal → Autonomous Work
- #06 Persistent State → Proactive Action
- #14 Agent Workspace + Governance Layer
- #17 AI Work Queue / Judgment Queue
- #22 Agent as First-class Identity
- #27 Context Belongs to Work

真正新增的不是这些组件本身，而是它们开始收敛成一个更高层产品关系：

> **用户不是委托一次 Task，而是在一定边界内把一段持续 Responsibility 委托给一个长期存在的 Agent。**

暂记为 Pattern Candidate：**Responsibility Delegation / Responsibility as Persistent Agent Contract**。不新增正式编号；需要至少另外两个独立产品证明它不是 OpenAI dots 的产品包装。

## 可迁移产品原理

### 1. 持久的应该是 Responsibility，不只是 Agent

Agent identity 只是“谁在工作”。真正有业务意义的是“它长期负责什么、边界是什么、什么时候算完成/失效”。

### 2. Proactivity 必须与 Authority 一起设计

越主动的 Agent，越不能只依赖 prompt。需要独立的权限、审批、safety review 和 lifecycle。

### 3. Human-in-the-loop 应从每一步确认迁移到边界与例外

如果每个动作都确认，长期 Agent 退化成慢速 Copilot；如果完全不确认，则责任与风险失控。产品核心是把人放到 consequential action / exception / policy boundary。

### 4. Reset / Pause 是 Agent-native 基础交互原语

长期 actor 必须有生命周期控制，而不仅是 Stop generation。

## 对本地 Agent 的启发

已有 Agent Runtime / CLI / browser / MCP / local files 时，可以直接复刻：

```text
Responsibility
→ Goal
→ Persistent Context
→ Trigger / proactive loop
→ Proposed Action
→ Policy Gate
→ Execute
→ Activity / Evidence
→ Feedback
→ Continue / Pause / Complete
```

值得做：
- Responsibility registry
- persistent agent identity
- explicit scope / policy
- activity timeline
- pause / resume / revoke
- action-time policy gate
- exception queue

不值得抄：
- 虚拟宠物、avatar 等人格包装，除非证明它显著改善长期责任关系。
- 4,000+ integrations 数量竞赛；工具数量不是机制。
- 为了“主动”而主动推送大量通知。

## 对应急管理 / B/G 的启发

专家 Agent 不应只接收“帮我检查这家企业”的一次性任务，可以形成：

```text
Responsibility: 持续负责某批责任主体的风险变化
├── scope: 企业/区域/风险类型
├── allowed evidence: 台账/视频/现场/历史隐患
├── authority: 可分析/可生成检查建议/不可签发正式文书
├── triggers: 新证据/风险变化/周期节点
├── exception: 高风险/证据冲突/需要正式执法
└── lifecycle: active / paused / transferred / closed
```

这比“周期任务 + 自动化”更接近长期专家代理：任务会结束，责任可以跨任务持续存在。

## 最大未知 / 反证

1. **最大的反证是用户行为。** 如果用户最终仍把 dot 当成一个更强的聊天窗口/通用 Agent，而不长期委托责任，则 Responsibility Delegation 只是营销叙事。
2. 多数底层能力高度可组合；机制价值主要来自产品对象和默认工作方式，而不是技术不可复制。
3. 当前只有一个 dot；真正的多 Agent responsibility allocation 尚未验证。
4. 长期主动 Agent 的错误累积、目标漂移、权限疲劳和 notification burden 可能显著削弱体验。
5. OpenAI 官方材料对“learns from feedback”的具体实现缺少技术细节。

## 跟踪问题

- 是否出现明确的 Responsibility / role / ownership 管理界面？
- 用户是否能看到 dot 当前承担的长期责任，而不只是 task list？
- 多 dots 是否支持责任转移、拆分、冲突解决？
- activity view 是否最终演化为以 outcome/exception 为中心，而不是流水日志？
- Custom Rules 是否从自然语言偏好升级为稳定 policy object？

## Sources

- OpenAI, Introducing dots, 2026-09-29: https://openai.com/index/introducing-dots/
- OpenAI, DevDay 2026 recap: https://openai.com/index/devday-2026-recap/
- OpenAI Help Center, Getting started with your dot (updated around 2026-10-05/06): https://help.openai.com/en/articles/20001530-getting-started-with-your-dot
- OpenAI Help Center, Dots privacy, security, and safety FAQs: https://help.openai.com/en/articles/20001529-dots-privacy-security-and-safety-faqs
- WIRED, OpenAI's Dots Are Always-On AI Agents, 2026-09-29.
- The Verge hands-on coverage, early October 2026.
