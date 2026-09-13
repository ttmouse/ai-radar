# Diiverge

**创新评分：★★★★☆**  
**模式：Generation as State Transition**

## 真实问题
大多数生成式产品把 AI 输出当一次性 artifact：Prompt → Image/Video → 结束。生成结果很少成为下一步交互的持久世界状态。

## 核心机制
用户从当前场景选择对象与行为 → AI生成下一状态和过渡 → 新状态被永久保存 → 自己或其他人可以继续从这个节点探索、分叉和生成。

## 为什么不是普通生成模型
单个 vision/image/video 模型都不新；新点是把 generation 从“生成一个东西”变成“改变一个可持续演化的 state graph”。

## Composable Test
技术组件可拼装，但产品原语发生变化，因此仍值得作为应用层创新观察。

## 可迁移原则
> Generation should sometimes be modeled as State Transition, not Disposable Output.

## 对产品设计的启发
AI PRD、风险分析、项目计划都可以不再是一次性文档，而是当前业务 State 的视图：新证据/新决策改变 State，报告、Dashboard、督办只是派生产物。

## 跟踪判断
**商业价值尚待验证，但机制值得保留。**