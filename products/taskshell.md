# TaskShell

**创新评分：★★★☆☆（观察）**  
**模式：Human-Agent Shared Task System**

## 原始亮点
把任务设计成可以由人或 Agent 领取、执行、提交审核、退回重做的共同工作对象。

## 互动后判断变化
你提出的关键反问是：**传统看板 + CLI + 自动化 + Agent，其实已经可以做到大部分效果。**

因此判断被修正：不能因为“Agent 能认领任务”就认为是新范式。真正需要看的，是任务对象本身有没有为 Agent 重构。

## Composable Test
Linear / GitHub Issues + CLI + MCP + 自动化，已经可以拼出：任务创建 → Agent 读取 → 执行 → 回写结果 → 人审核 → 退回继续执行。

所以如果 TaskShell 只是把这套能力包装得更顺，它主要是产品化创新。

## 真正值得看的部分
如果 Task Schema 开始稳定承载以下内容，才更接近 Agent-native：

- Goal
- Acceptance Criteria
- Context
- Tools
- Permissions
- Execution History
- Artifacts
- Human Checkpoints
- Retry Strategy
- Agent Identity

传统 Task 主要服务“人知道要做什么”；Agent-native Task 需要进一步回答“系统如何知道它做完了，以及失败后如何继续”。

## 对本地 Agent 的启发
优先复用现有任务系统，不要先重做一个 AI 看板。先补齐 Agent 执行需要的字段，再判断是否真的需要新产品形态。

## 可迁移原则
> 不要为了 Agent 重做 UI；先检查现有业务对象缺少哪些“可执行字段”。

## 不值得抄
- AI 自动认领任务本身不算创新。
- 多 Agent 看板不自动等于新范式。
- 不要把聊天 Session 当成 Task State。

## 跟踪判断
**继续观察。** 真正值得跟踪的是 Task Schema 是否发生变化，而不是看板里是否出现 AI。