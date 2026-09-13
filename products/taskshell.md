# TaskShell

**创新评分：★★★☆☆（观察）**  
**模式：Human-Agent Shared Task System**

## 原始亮点
把任务设计成可以由人或 Agent 领取、执行、提交审核、退回重做的共同工作对象，尝试让 Agent 进入传统任务系统。

## Composable Test 结果
这一机制很大程度可以用 Linear/GitHub Issues + CLI + MCP + 自动化完成，因此不能仅因为“Agent 能认领任务”就判断为新范式。

## 真正可能有价值的部分
如果它把 Task Schema 重构为 Agent-native 对象，才值得继续看：Goal、Acceptance Criteria、Context、Tools、Permissions、Budget、Execution History、Artifacts、Human Checkpoints、Retry Strategy、Agent Identity。

## 可迁移原则
> 不要为了 Agent 重新造一个看板；先问现有任务对象缺少哪些 Agent 执行所需的字段。

## 对本地 Agent 的启发
优先复用 GitHub/Linear/现有任务系统，把 Agent Runtime 与其连接；只有当 Agent 需要的状态无法承载时，再扩展 Task Schema。

## 跟踪判断
**观察，不再把“Agent任务看板”本身视为强创新。**