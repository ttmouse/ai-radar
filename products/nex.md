# Nex

**创新评分：★★★★☆**  
**模式：Deterministic / Probabilistic Split**

## 真实问题
很多“Agent 自动化”把每一步都交给 LLM，导致成本高、速度慢、结果不稳定；但纯规则系统又处理不了开放判断。

## 核心机制
先让 AI 理解工作并生成/组织可重复执行的确定性逻辑；运行时只有真正需要语义判断、分类、生成或例外处理的部分才调用模型。

## 为什么不是普通加 AI
重点不是给自动化流程增加 LLM 节点，而是重新划分 AI 与软件的职责边界。

## Human / AI / Software 分工
- Software：重复、确定、可验证的步骤
- AI：开放判断与不确定部分
- Human：高责任例外和最终批准

## Composable Test
技术上可以用代码生成 + workflow engine + LLM routing 复刻，因此壁垒不在组件；但这个架构原则对真实 Agent 产品极其重要。

## 可迁移原则
> Deterministic → Software；Probabilistic → AI；High-stakes → Human。

## 对本地 Agent / B/G 的启发
不要把后台数据查询、状态流转、权限判断、固定通知都做成 Agent 推理。它们应该留在确定性程序里，让 Agent 只负责理解、判断与异常。

## 跟踪判断
**持续跟踪，尤其作为架构基准。**