# AIR Security

**创新评分：★★★★☆**  
**模式：Context Supply Chain**

## 真实问题
Agent 的风险不只来自模型和权限，还来自进入 Context 的 Skill、MCP、网页、插件、外部数据与 sub-agent。任何一环被污染，都可能影响后续行为。

## 核心机制
把 Agent 依赖的上下文来源视为一条动态供应链，持续检查来源、信任、变化和风险，而不是只做静态 RBAC。

## 为什么不是普通安全功能
传统权限回答“你能访问什么”；Context Supply Chain 还要回答“你现在信任的这段信息从哪里来、是否被修改、是否值得进入推理”。

## Composable Test
安全扫描器、allowlist、签名都能拼装，但持续 Context trust chain 是 Agent 时代新的治理对象。

## 可迁移原则
> Agent 安全不仅是 Action Security，也是 Context Security。

## 对本地 Agent / B/G 的启发
本地 Agent 的 Skill、MCP、网页内容、企业上传材料、外部政策文件应带来源和信任等级；高责任判断不能把未经验证的 Context 与权威数据等价处理。

## 跟踪判断
**值得持续跟踪。**