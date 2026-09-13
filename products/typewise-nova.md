# Typewise Nova

**创新评分：★★★★★**  
**模式：Agent Operations Agent**

## 一句话
Nova 不是客服 Agent，而是**运营其他 Agent 的 AI Operator**。

## 真实问题
生产 Agent 上线后会持续出现 prompt decay、knowledge 过期、tool/API 变化、模型漂移、失败模式和新业务规则。真正难的不是“创建 Agent”，而是长期维护 Agent。

## 核心机制
Observe → Diagnose → Propose Change → Simulate → Evaluate → Human Approval → Deploy → Observe again。

Nova 读取运行轨迹、失败案例、知识和规则，发现性能变化，提出修改方案，用历史案例做回归测试，再把差异交给人批准。

## 为什么不是普通加 AI
自然语言创建 Agent 很容易被现有 Coding Agent 复刻；Nova 真正变化的是把 Agent 本身当成需要持续运营的软件资产，并引入 AI 负责运维。

## Human / AI / Software 分工
- Worker Agent：执行业务
- Operator Agent：发现问题、提出改动
- Eval/Software：稳定测试、指标、版本、回滚
- Human：批准高风险变化与治理边界

## 可迁移原则
> Agent 必须有自己的 CI/CD：Trace → Eval → Diagnose → Improve → Test → Approve → Deploy → Rollback。

## 对本地 Agent 的启发
最小版本无需做多 Agent：先保存执行轨迹、人工评分、eval set、prompt/skill version、regression runner 和 diff；然后再让 Operator 自动聚类失败、提出 patch。

## 不确定性
壁垒不在 Demo，而在长期 Eval 数据、失败分类和安全的 Change Policy。

## 跟踪判断
**最高优先级跟踪。**