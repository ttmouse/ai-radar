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

## 跟踪判断
**强烈值得跟踪。**