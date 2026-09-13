# Switch

**创新评分：★★★★☆**  
**模式：Context Belongs to Work**

## 真实问题
团队成员分别在 Claude、Codex、ChatGPT、Slack 中工作，Context 被锁在不同工具和 Agent 身上，人承担同步成本。

## 核心机制
把 Room/项目/任务设计成持久 Context Container：成员、资源、历史、规则属于工作空间；不同 Agent 可加入、退出或替换，而工作状态仍然存在。

## 为什么不是 Slack Bot
把 Agent 接到 Slack 很普通；真正新的是 Agent-neutral Workspace。Agent 是可替换计算资源，Work Context 才是长期资产。

## 可迁移原则
> Context belongs to Work, not to Agent.

## 对本地 Agent / B/G 的启发
项目、企业、周期任务、隐患应成为 Context Container；Agent 临时加入读取上下文、完成工作、写回 Artifact/State 后退出，而不是给每个 Agent 积累孤立 Memory。

## 跟踪判断
**值得持续跟踪。**