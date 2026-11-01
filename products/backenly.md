# Backenly｜Agent-facing backend + governed autonomous maintenance

> **研究日期：2026-11-01；首次观察：2026-10-31 Product Hunt。状态：条件 Core（需要真实运行验证）；Tracking 5/5；Mechanism Novelty 3/4（组合型结构变化）；Evidence B（开源代码可检查，未做独立部署/生产验证）；新 Pattern：否。**

## 0. 先了解产品

- 官网：https://backenly.com/
- 开源代码：https://github.com/backenly/backenly
- 架构说明：https://github.com/backenly/backenly/blob/main/AGENTS.md
- 运行时验证：https://github.com/backenly/backenly/blob/main/lib/changes/verify.ts
- 自动化级别：https://github.com/backenly/backenly/blob/main/lib/autonomy/autonomy-level.ts
- 后置条件验证：https://github.com/backenly/backenly/blob/main/lib/autonomy/action-postconditions.ts
- 活跃项目识别：https://github.com/backenly/backenly/blob/main/lib/autonomy/activity-gate.ts
- 2026-10-31 发布：https://www.producthunt.com/leaderboard/daily/2026/10/31

**定位**：面向用 Claude Code/Codex/Cursor 等 Coding Agent 构建应用的开发者，提供可由 MCP 驱动的后端（PostgreSQL、PostgREST、认证、存储、实时订阅、函数），以及后端上线后持续检测和修复的控制层。官方宣称完整开源、可自托管。用户不是先学习一个新的后端管理 UI，而是让既有 Coding Agent 申请和实施后端变更；平台负责受治理的执行、核验和后续维护。

**典型任务**：开发者要求 Coding Agent 为订单系统新增 orders/order_items 表、RLS 权限与 REST 接口；Agent 经 MCP 提交结构化操作；平台对结构变更做预演、审批判定、执行、真实数据库读回、版本/审计；上线后定期扫描可自动修复的缺陷，将危险操作留给人决定。

## 1. 真实问题 / Before → After

旧流程：
1. Human 描述业务，Coding Agent 写 SQL/migration、接口、权限。
2. Human 或 CI 执行迁移，手动检查 schema 与 RLS。
3. 上线后由人看监控、查缺失索引/错误配置、提交修复；风险变更走 PR/DBA 审批。
4. Agent 的「已执行」与真实后端状态、生产安全性之间可能脱节。

新流程（厂商设计）：
1. Human 给意图和风险边界；Coding Agent 通过 MCP 读取当前后端状态并提出变更。
2. **Governed Change** 将请求转为有身份、计划、权限、影响范围、版本、回滚点的受控动作。
3. **Deterministic Software** 负责在真实数据库执行、检查副作用和业务安全不变量；不可验证不得当作成功。
4. 后台观察实际 schema、流量和性能，检测已知可修复的缺陷；可逆低风险操作自动修复，危险操作形成审批请求。
5. Human 从手动操作员变为高风险变更的裁决者和最终责任人。

**Mechanism Delta**：后端由「供人/Agent 写入的一组 API 和数据库」转向「由 Agent 请求改变、由独立控制层验证并持续维护约束的业务运行对象」。这是工作责任的转移，而不是 MCP 接入本身。

## 2. 可检查的产品机制

### A. 一个受治理的变更内核
官方 AGENTS.md 指向 `lib/changes/ledger.ts`、`lib/changes/verify.ts`：变更前快照、执行、后置验证、可追溯事件与必要时恢复。验证规则由平台控制，不能由 Agent 请求参数弱化。源代码 `verify.ts` 返回 `confirmed / contradicted / inconclusive`，区分成功、失败与无法确认；检查包括预期效果、连带删除、RLS、凭据表暴露、API 可访问性、认证契约、索引与表查询。

### B. 可自动修复的安全区与人工审批边界
`lib/autonomy/autonomy-level.ts` 定义 OFF / CONSERVATIVE / BALANCED / AGGRESSIVE。Tier 0/1 才可能自动执行；Tier 2+（认证、外部凭据、破坏性/不可逆等）硬性需要 Human Approval，用户的档位不能越过此边界。官网称自愈中的常见修复是规则驱动，不应误称每分钟都有 AI 自主推理。

### C. 可证伪的修复结果
`lib/autonomy/action-postconditions.ts` 的说明明确：只有目标动作的数据库后置条件、所属探针重新检查与回归检查共同支持，才能将 AI 提议的修复标为 fixed。读取失败是 `error`，不是 `confirmed`。注意此文件目前可见的明确后置条件登记仅包含 `ADD_COLUMN`；不能由此推断所有修复类型都已有同等验证覆盖。

### D. 以真实活动而非聊天活动判定是否需要维护
`lib/autonomy/activity-gate.ts` 记录过一项设计错误：旧逻辑只看 30 天内是否有 ConversationMessage，导致仅通过 MCP 工具调用的项目或持续服务用户但无人聊天的生产项目可能停止自愈。代码注释举例称某项目 47 天未发消息而失去维护。新逻辑纳入真实 API 流量、受治理变更、会话与项目创建时间。**这是源码注释中的厂商自述，不是独立事故报告。**

## 3. Composable Test v2

- **1–2 天可拼的部分**：Supabase/Postgres + MCP + LLM 生成迁移 + Git/数据库快照 + cron 探针 + Approval UI，可以实现单项目「描述→建表→检查→审批→补索引」演示，属于已有能力组合。
- **不能等同复刻 80% 稳定产品机制的部分**：多类变更统一经过不可绕过的执行内核；失败时不丢数据的恢复；权限和身份强制约束；所有结果必须从真实运行时读回；持续修复中对误报、权限漂移、循环修复的控制。
- **结论**：Composable but Structural。结构增量主要是后端**持续运营责任**由人转向确定性软件，Human 转为风险裁决；不过已有 Pattern 能覆盖其构成机制，因此**条件 Core，不新建 Pattern**。若部署后只剩下「Agent + 数据库控制台 + 定时脚本」，应降级 Observe。

## 4. Human / AI / Deterministic Software 分工

| 角色 | 旧职责 | 新职责 |
|---|---|---|
| Human | 设计 schema、审核/执行变更、巡检和修复 | 定义业务目标与不可突破的风险边界；批准危险变更；承担生产责任 |
| AI | 辅助生成 SQL、接口、修复建议 | 将业务意图转为结构化变更提议，辅助定位不确定性问题 |
| Deterministic Software | 存储、执行 SQL、提供监控 | 持有真实状态、强制执行变更内核、验证不变量、限制自治等级、生成可审计证据、自动修复可证实的安全问题 |

**注意**：自愈核心的一部分是规则系统，而不是模型。将所有自动修复归功于 AI 是错误归因。

## 5. 事实 / 合理推断 / 未知 / 分析判断

**已确认（代码/公开资料）**：开源仓库包含独立的变更验证器、自治档位、后置条件验证器、项目活动门禁和 MCP 工具设计；代码明示三态验证、Tier 2+ 不自动批准；官方披露专用 PostgreSQL cluster 需求与 PostgREST 配置限制。

**合理推断**：统一变更内核 + 真实状态读回，比直接给 Agent 数据库 DDL 权限更容易形成审计与恢复边界；以运行时流量决定是否维护，比以聊天活跃度决定更符合生产系统的生命周期。

**未知**：真实用户的生产修复成功率、误修复率、长期稳定性、各修复类型覆盖率、备份恢复成功率、权限边界的攻防验证、并发下的数据一致性、复杂多仓库应用适配情况。没有自行部署或运行测试。

**最大风险/反证**：
1. 官方将自动修复称为安全且可逆，但真正安全依赖检测与回滚是否覆盖全部副作用。代码存在保护措施，不等于保护已通过第三方安全审计。
2. `autonomy-level.ts` 对「读取 Project 记录失败」采用 `AGGRESSIVE` fallback；虽然 Tier 2+ 仍强制人工审批，但这构成需要进一步测试的**配置不可读时是否保持用户 OFF 意图**的问题。不能仅凭这一行代码断言已发生权限绕过。
3. 开源 README 承认 PostgREST 在共享 PostgreSQL cluster 上可能出现 schema 注册冲突，推荐专用 cluster；生产部署并非零运维。
4. 自愈循环可能产生修复震荡，需验证幂等、熔断、重复告警、回滚与并发控制。
5. 「schema 与权限技术正确」不等于「业务模型符合真实需求」。

## 6. 与现有 Pattern 的最近邻比较

| 已有 Pattern | 已覆盖内容 | Backenly 的具体增量 |
|---|---|---|
| #05 AI Build → Software Run → AI Repair | AI 构建确定性流程，软件运行和修复 | 扩展到真实数据库与后端维护的持续生命周期 |
| #13 Deterministic / Probabilistic Split | 确定性规则与模型分工 | 明确自愈安全区和不可验证状态 |
| #14 Agent Workspace + Governance | Agent 与治理层分离 | 强制统一受治理变更内核 |
| #15 Proposed Action | 先提议后审批 | 将数据库影响范围、快照、恢复与审批绑定 |
| #17 Judgment Queue | 人处理例外 | 高风险变更才占用人的注意力 |
| #26 Agent-facing System of Record | 可信业务状态与 Agent-facing API | 直接把后端运行时当作可被治理的执行层 |
| #29 Runtime + Source + Telemetry | 真实运行证据进入 Agent 行动 | 用运行时探针判断是否修复，并校验修复后结果 |

**它真正新增什么？** 在一个可审计的开源实现中，将这些已有模式落实为「Backend Change / Guarantee / Finding / Approval / Verification」的长期对象链。不是尚未见过的新抽象模式，因此正式 Pattern 仍为 30。

## 7. 可迁移到本地 Agent / AI 原生研发 / 应急管理

**本地 Agent 可复刻**：不要先换数据库。用 SQLite/Postgres + 现有 CLI/MCP 建一个最小 `GovernedAction`：`intent → proposed_action → risk_tier → precondition → human_approval? → execute → independent_postcondition → evidence → rollback`。先限定三类低风险可逆动作，记录真实误判与恢复数据。

**AI 原生研发**：将「Agent 宣称迁移完成」替换为「数据库 catalog、API 和权限规则读回确认」；保留 `inconclusive`，不要把探针超时当成通过。

**应急管理 B/G**：可将此模式迁移为隐患闭环的控制结构：AI 发现风险/建议整改，规则系统校验检查点、证据、责任主体、时效与授权；低风险提醒可自动触发，风险等级变更、正式督办、隐患关闭必须按规则或专家裁决。自动复核不能靠同一个 AI 对自己生成的证据打分。

**不值得抄**：从零重写 Supabase/PostgREST、自己造数据库安全引擎、以每分钟自愈频率当 KPI、让 LLM 直接执行任意 SQL、在没有可验证不变量前开启自动修复。

## 8. 最小验证与后续跟踪

用隔离测试库，准备 12 个用例：4 个安全可逆缺陷（缺失索引、可恢复配置漂移等）、4 个必须审批的危险变更、4 个异常（数据库不可读、探针失败、重复修复、用户设 OFF）。比较自动正确修复率、错误放行数、不可验证被误报为成功数、回滚成功率和人工耗时。

**继续跟踪：5/5。条件 Core 保留条件**：必须获得独立部署、实际执行/恢复测试或生产案例；若发现风险门禁不成立、误修复较多、只是既有工具组合而无持续责任变化，则降级。下次研究只更新此档案，不重复创建产品。