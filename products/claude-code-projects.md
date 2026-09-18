# Claude Code Projects

**日期：2026-09-17**  
**创新评分：★★★★☆**  
**判断：核心产品入选，但不新增 Pattern**

## 产品入口
- 官方产品：Claude Code / Anthropic
- 参考报道：VentureBeat, 2026-09-17, “Anthropic launches Claude Code Projects, an always-on conversation that remembers and delegates your long-running dev work”

## 1. 真实问题
Coding Agent 已经能完成单个任务，新的瓶颈变成：一个持续数周甚至数年的项目里，谁保存目标、历史决策、依赖和当前状态？谁决定新要求应该交给哪个执行 Session？如果用户仍需自己拆任务、重建 Context、盯多个 Agent，人的工作只是从“写代码”变成“调度 Agent”。

## 2. 过去工作流
用户维护一个项目目标，但每次启动新的 Coding Agent Session 都需要重新提供 Context；多个 Session 并行时，用户自己负责拆任务、分配、跟踪、汇总和处理依赖。Session 是核心工作单位，Project 只是外部容器。

## 3. 新工作流
Claude Code Projects 把一个持续存在的 Project Conversation 放到执行 Session 之上。用户持续向这个 Coordinator 描述目标和变化；Coordinator 判断是直接回答、路由到已有 Thread，还是新建 Thread。Thread 是完整 Claude Code cloud session，有独立 branch / repo copy，可继续拆 subagent；完成后把状态回报 Project。Project Memory、instructions、files 等作为跨 Thread 的持续 Context。

核心结构：

Human → Persistent Project / Coordinator → Worker Threads → Branch / PR / CI → Project State → Human attention

## 4. 最关键机制
真正重要的不是 parallel agents，而是把“Project”从文件夹/聊天集合升级为 **Persistent Coordination Object**：它同时承载长期目标、共享 Context、工作线程、状态和人的注意力入口。

用户的主要交互对象从“某个 Agent Session”上移为“持续存在的工作本身”。

## 5. Composable Test
并行 Coding Agent、git worktree、branch、CI、MEMORY.md、任务队列都可以在 1–2 天拼出来，所以这些单独都不算创新。

Claude Code Projects 仍值得入选，是因为它把这些能力组合成一个新的一级交互对象：用户不再直接管理 Session，而是持续 brief 一个 Project-level Coordinator，由系统决定 Session 的创建、路由和延续。

但它没有通过“新增 Pattern”的门槛，因为这一机制主要是 Pattern 27 Context Belongs to Work + Pattern 03 Goal → Autonomous Work + Pattern 17 Judgment Queue 在 Coding 场景的成熟组合。

## 6. Human / AI / Deterministic Software 分工
- Human：维护目标、改变要求、处理真正需要判断/审批的节点。
- Coordinator AI：解释新输入、维护项目级上下文、决定任务路由与 Thread 创建。
- Worker AI：执行具体代码任务，可继续委派 subagent。
- Deterministic Software：Git branch、repo clone、PR、CI、merge conflict、usage limits、状态持久化。

关键变化：Human 从 Session Scheduler 上移到 Project Director。

## 7. Runtime / Context / Memory：事实与推断
### 已确认事实
根据公开报道：每个 Thread 是完整 cloud Claude Code session，有自己的 branch 和 repository copy；Project 有 shared memory；Thread 可并行并继续拆 subagent；Overview 按 Ready for review / Waiting on you / Working / Landing / Idle / Resolved 等状态聚合；Thread 可以跟踪 PR / CI 并继续修复。

### 合理推断
Coordinator 应维护一个比单个 Thread context 更压缩的 project state，并根据 Thread 回报更新它；否则长期项目很快会遇到 Context 膨胀。但公开材料不足以确认其内部 state representation、memory consolidation、conflict resolution 和 routing policy。

### 未知
Project Memory 长期漂移如何控制；错误决策如何撤销；跨数百 Thread 的历史如何压缩；Coordinator 如何评价 Thread 是否真的完成业务目标；多个 Thread 产生语义冲突时是否只依赖 Git merge conflict。

## 8. 与历史 Pattern 比较：真正新增了什么
不是新 Pattern。

- Pattern 03 Goal → Autonomous Work：已有持续目标执行。
- Pattern 17 Judgment Queue：Overview 已把人的入口转为 Waiting on you / Ready for review。
- Pattern 27 Context Belongs to Work：最接近。Context 从 Session 上移到 Project。
- Pattern 28 Generation as State Transition：Thread 的执行持续改变 Project 状态。

它真正新增的是一个很成熟的产品实现证据：**Project，而不是 Agent / Chat / Session，正在成为 AI 原生软件的一级对象。**

## 9. 对产品设计 / 本地 Agent / B/G 的启发
这与长期目标 Agent 方向高度相关。长期目标不应该等于“一个永远不结束的聊天”。更合理的是：

Project = Goal + State + Memory + Workstreams + Artifacts + Decisions + Human Attention Queue

Agent Session 只是 Project 临时创建的执行资源。

对应应急专家工作台：一个“本周期完成某批责任主体检查”的目标，可以成为持续 Project；AI看、视频看、现场看、隐患验收是 Workstreams，而不是彼此割裂的产品入口。Project 保存目标、周期状态、证据和待人工判断事项。

## 10. 本地可复刻路径
现有 Runtime / CLI / 浏览器 / MCP / 本地文件已经足够做 V0：
1. project.json 保存 goal / status / constraints；
2. memory.md 保存长期决策，不保存完整聊天；
3. workstreams/ 保存子任务及依赖；
4. 每个 Worker Session 独立执行并返回 structured result；
5. Coordinator 只读 Project State + Worker summary，负责创建/路由任务；
6. attention_queue 只收集 blocked / approval / conflict / review；
7. Git / filesystem 做确定性 artifact state。

不值得抄：为了“多 Agent”而多 Agent；把所有 Thread 原始对话塞进共享 Context；自己重新实现 Git/CI；让 Coordinator 参与每个低层工具调用。

## 11. 最大未知与反证
最大的反证是：这可能只是“Linear + git worktree + Claude Code + shared memory”的优质整合，而不是新产品机制。并且 Coordinator 本身可能成为新的单点瓶颈：它若错误理解 Project State，会把错误扩散给所有 Worker。

另一个风险是长期项目 Memory 的可信度。Project 活得越久，“记住更多”并不等于更好；真正困难的是遗忘、版本、冲突和可追溯性。

## 12. 跟踪判断
**4/5，继续跟踪。**

不是因为 Multi-Agent，而是因为它进一步证明：AI 原生工作软件的一级对象正在从 Chat / Agent Session 上移为 Persistent Work / Project。