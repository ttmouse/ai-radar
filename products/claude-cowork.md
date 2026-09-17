# Claude Cowork

**创新评分：★★★★☆（历史）**  
**模式：Goal → Autonomous Work**

## 核心判断
Cowork 的历史价值不在“Claude 会做更多工具调用”，而在从一次性答复转向持续工作：用户给出目标，Agent 在工作空间中推进任务，在真正需要判断时再让人介入。

## 工作流变化
传统：人拆步骤 → AI逐步回答 → 人搬运结果。  
Cowork：人定义目标/边界 → Agent 自主推进 → 关键节点 Human-in-the-loop → 交付 artifact。

## Composable Test
与 ChatGPT Work 属于同一大模式，因此不单独创造新产品原理。差异更多在执行环境、工具生态、权限和协作体验。

## 2026-09-16 产品变化：Cowork 被折回 Claude
Anthropic 开始取消 Chat 与 Cowork 的显式模式选择。用户不再需要预先判断“这是一个聊天问题，还是一个需要 Agent 长时间执行的任务”；同一个 Claude 对话可以从快速问答自然升级为多步骤项目，系统根据任务需要调用 context、skills、connectors 和长时执行能力。原 Cowork 的 chats、projects、artifacts、connectors、skills 继续保留，但 Cowork 作为独立产品表面逐步消失。

同时 Claude Docs、Claude Slides 和 Design 成为同一对话中可生成、继续编辑和协作的工作对象，而不是聊天结束时导出的静态文件。

## 这次变化真正值得记录的地方
这不是一个新的一级 Pattern，而是 Pattern 03 的成熟：**用户只表达 Intent，系统自己决定执行深度与工作形态。**

旧产品结构：

```text
用户先选 Chat / Cowork / Design
→ 再表达需求
→ 产品按模式执行
```

新结构：

```text
用户表达 Intent
→ 系统判断是 Answer / Agentic Work / Doc / Slides / Design
→ 必要时自动升级执行深度
→ 产物保持在同一 Context 中继续演化
```

这减少的是一种很隐蔽的用户负担：**要求用户理解 AI 产品内部的能力边界，然后替系统做路由。**

## Human / AI / Software 分工变化
- Human：表达目标、约束、判断结果是否可接受；不再负责预选“该用哪个 AI 模式”。
- AI：理解 Intent，并决定回答、长任务、文档、幻灯片或设计等执行路径。
- Deterministic Software：维护对话、项目、artifact、connector、permission、sharing 与协作状态。

## 对本地 Agent / B/G 的启发
你本地的 Agent 工作台也应警惕过早暴露“AI 看 / 视频看 / 现场看 / 深度任务 / 多 Agent”等内部能力模式给用户。用户真正应该选择的是业务目标或任务；系统再根据目标路由到合适能力。

但这不能机械套用。应急等高责任流程里，检查方式、证据来源或法定流程如果本身具有业务含义，就不能为了界面简单而隐藏。应该隐藏的是**技术执行模式**，不是业务责任边界。

## 可迁移原则
> 当不同 AI 模式只是系统为了完成同一用户 Intent 所采用的执行策略时，不应该要求用户先选择模式；让系统路由。只有模式本身具有业务、权限或责任意义时，才应该显式暴露。

## 跟踪判断
**继续跟踪，但 2026-09-16 不新增模式编号。** Cowork 的独立产品表面消失，本身反而验证了一个成熟方向：Agent 能力逐渐从“一个模式”变成软件默认运行方式。