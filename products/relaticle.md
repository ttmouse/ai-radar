# Relaticle

**创新评分：★★★★☆**  
**模式：Proposed Action as First-class Object**

## 真实问题
企业 Agent 一旦拥有 CRM/ERP 写权限，“弹一个确认框然后执行”仍然太粗糙。用户需要知道 AI 想改什么、为什么改、依据是什么、风险在哪里。

## 核心机制
把 AI 写操作先转成独立的 PendingAction / ProposedAction：保存 before state、proposed state、reason、evidence、risk、approval status。它本身成为可审计业务对象。

## 为什么不是普通加 AI
变化不在模型，而在业务对象模型：AI 的意图不再直接等于系统动作。

## Composable Test
API + approval dialog 很容易做，但如果没有 Action 对象、生命周期和审计，就无法形成稳定治理层。

## 可迁移原则
> AI 的高责任动作应该先“成为对象”，再决定是否执行。

## 对本地 Agent / B/G 的启发
生成督办、调整风险等级、关闭隐患、修改企业归属等都应先产生 Proposed Action，展示证据与差异，再由对应角色批准。

## 跟踪判断
**强烈值得跟踪。**