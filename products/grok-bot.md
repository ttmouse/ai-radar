# Grok Bot

**创新评分：★★★★☆**  
**模式：Agent Workspace**

## 真实问题
复杂 Agent 若每次任务都从空环境开始，就无法自然保留登录状态、文件、浏览器历史和执行中的中间产物。

## 核心机制
给 Agent 一个持续存在的计算/浏览环境，使工作状态不只存在模型上下文窗口里，而存在真实 Workspace 中。

## 为什么重要
Memory 不应只理解为向量库或聊天摘要。文件系统、浏览器 session、应用状态、终端进程也都是 Agent 的外部记忆。

## Composable Test
VM + browser + model 可以组合，因此创新更多在产品集成；但“Workspace as Memory”是长期 Agent 的基础机制。

## 可迁移原则
> Agent 的持久性来自工作环境和业务状态，不只是更长的 Context Window。

## 对本地 Agent 的启发
你的本地项目目录、浏览器 Cookie、CLI、任务状态天然构成 Workspace，可以比云 Agent 更容易获得稳定持久 Context。

## 跟踪判断
**作为 Runtime 参考持续观察。**