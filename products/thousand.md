# Thousand

**创新评分：★★★★☆**  
**模式：Agent as First-class Identity**

## 真实问题
很多 Agent 直接借用人的 token 或系统万能账号。这样无法清楚回答：这个动作究竟是谁做的、它能访问什么、权限何时过期、怎样单独撤销。

## 核心机制
把 Agent 当成独立身份主体：拥有自己的 identity、scope、resource permission、expiry、audit trail，而不是“代表某个人的脚本”。

## 为什么不是普通权限系统
RBAC 仍然重要，但 Agent 是长期运行、可复制、可委派的执行者，需要独立生命周期和责任边界。

## 可迁移原则
> Agent 进入生产系统后，应像服务账号/员工一样拥有独立身份，而不是借用人的权限。

## 对 B/G 的启发
建议权限栈：Agent Identity → Resource Permission → Context Permission → Action Policy → Human Approval → Audit。应急系统中不同 Agent 只能看到对应属地、企业和任务数据，并且写操作再受 Action Policy 限制。

## 跟踪判断
**高价值治理模式。**