# OpenMarket

**创新评分：★★★★★**  
**模式：Adversarial Agents + Independent Referee**

## 真实问题
单一推荐 Agent 同时负责搜索、理解、评价和给结论，容易把假设、偏好和验证混在一起；尤其购物、采购、投资等场景天然存在利益冲突。

## 新工作流
Buyer Agent 表达需求 → 多个 Seller Agents 从不同立场提出主张并互相挑战 → Independent Referee 检查证据 → 人做最终选择。

## 核心机制
不是 Multi-Agent 本身，而是把**利益冲突和反驳机制编码进系统架构**。

## 为什么不是普通加 AI
普通 AI shopping 只是“搜索 + 排名 + 推荐理由”；OpenMarket 改变了决策制度：Proposer、Challenger、Verifier、Decision Maker 分离。

## Composable Test
可以用多个模型角色快速做 Demo，但真正价值在独立证据、角色利益约束和裁判机制是否真实分离。

## 可迁移原则
> 不要让同一个 AI 同时提出观点、证明观点并裁决自己。

## 对 B/G 的启发
风险等级上调、重大隐患判断、方案评审可采用：Risk Agent 提议 → Challenge Agent 找反证 → Evidence Agent 验证来源 → 专家/领导最终决策。

## 跟踪判断
**强烈值得跟踪。**