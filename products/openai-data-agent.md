# OpenAI Data Agent

**创新评分：★★★★☆**  
**模式：Business Semantic Layer**

## 真实问题
自然语言查 SQL 已经不新。企业真正困难的是：不同系统中的字段、指标和关系代表什么，组织对“客户、风险、完成率、收入”等概念是否有统一口径。

## 核心机制
Agent 不只连接数据仓库，还消费业务术语、指标定义、计算规则、实体关系以及行列权限。AI 工作在“数据 + 组织语义”之上。

## 为什么不是普通 BI + AI
如果只是 NL2SQL，很容易复刻；真正变化是把 Semantic Layer 作为 Agent 的共享 Context，让分析、Dashboard、后续行动使用同一业务含义。

## 可迁移原则
> Don't expose the database. Expose the organization's meaning of the database.

## 对 B/G 的启发
应急系统应明确重大/较大风险、周期任务完成率、隐患闭环、逾期、组织归属、专家责任等统一语义。所有 Agent、报表、督办和分析都引用同一层，而不是各自解释。

## 跟踪判断
**值得跟踪，重点在语义治理而不是 SQL 能力。**