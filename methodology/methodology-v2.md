# AI Product Radar 研究方法 v2

> 目标不是发现更多 AI 产品，而是持续提高“识别软件组织方式变化”的能力。

## 0. 本轮复盘结论

v1 的方向正确，但存在五个系统性缺陷：

1. **Composable Test 权重过高**：可拼装不等于没有产品创新。Granola 类创新可能技术可组合，但重新定义了默认工作流或交互原语。
2. **Pattern 过早增殖**：当前 30 个 Pattern 中混合了“稳定模式、机制候选、架构原则、具体实现”，长期会产生同义模式和编号通胀。
3. **只问“新不新”，不够问“是否成为默认工作方式”**：Demo 创新和稳定产品机制没有分开。
4. **Product 是主要记录对象，Finding/Evidence 不是一等对象**：后续新证据、反证和判断变化难以结构化积累。
5. **评分过于单维**：1–5 容易把商业价值、技术难度、机制新颖性和证据成熟度混在一起。

因此 v2 不推翻 v1，而是在其上增加：**Mechanism Delta、Object/Responsibility Shift、Default Workflow、Evidence Maturity、Pattern Promotion**。

---

## 1. 不变的 L1 原则

这些原则默认不因每日候选而修改：

- 商业价值大 ≠ 产品机制创新强。
- 技术复杂 ≠ 产品机制创新强。
- Multi-Agent / MCP / Browser Agent / Memory / Automation 本身都不构成创新。
- 模型能力提升不是应用层产品机制创新。
- 必须回答：**因为 AI 的存在，软件的基本组织方式究竟发生了什么变化？**
- 必须区分事实、合理推断、未知和分析判断。
- 原始证据优先于媒体转述。

---

## 2. v2 核心判断框架：先找 Mechanism Delta

不再从“这个产品有什么 AI 功能”开始，而从下面的问题开始：

> 如果把 AI 从这个产品里拿掉，哪个原本不存在或无法成立的工作对象、责任关系、交互原语、控制结构或持续状态会消失？

输出一个明确的 **Mechanism Delta**：

```text
Before
→ AI-induced structural change
→ After
```

如果无法用 1–3 句话说清 Delta，默认不得进入 Core。

### Delta 的七类主要位置

1. **Work Object**：出现新的第一等业务对象。
2. **Responsibility**：责任从 Human / Software / Agent 之间重新分配。
3. **Authority**：身份、权限、审批、委托、审计结构变化。
4. **Interaction Primitive**：输入、反馈、控制 Agent 的基本动作变化。
5. **Context / Memory**：上下文归属、形成、更新方式变化。
6. **Persistence / Proactivity**：从一次性交互变成持久状态与主动行动。
7. **Evidence / Judgment / Attention**：证据获取、验证和人的注意力位置变化。

技术架构只有在支撑上述变化时才进入产品机制判断。

---

## 3. Composable Test v2：从否决器改成压力测试

### 旧规则的问题

“1–2 天能拼出 80% → 默认不创新”过于粗糙。

它会把两件事混在一起：

- **Implementation Composability**：技术组件容易拼。
- **Product Mechanism Novelty**：产品是否建立新的默认工作方式。

### 新规则

仍然执行 1–2 天 / 80% 测试，但继续追问三件事：

**A. 拼出来的是 Demo，还是稳定工作方式？**

如果只能复制 UI/Happy Path，而不能复制持续状态、权限、证据、失败恢复、多人协作或长期 Context，则不能声称复制了 80% 的“产品机制”。

**B. 组合后是否仍需要重新定义一个一等对象？**

例如 Project、Responsibility、Proposed Action、Agent Identity、Judgment Queue。若答案为是，则“组件可组合”不能直接否定机制创新。

**C. 用户是否必须改变原有工作习惯才能获得价值？**

如果只是把旧步骤自动执行，创新弱；如果旧工作流本身被删除、合并或责任重新分配，则继续评估。

结论标签改为：
- **Composable / Commodity**：组合即等价，Reject。
- **Composable but Structural**：技术可拼，但产品结构有新 Delta，继续评估。
- **Non-trivial System**：核心机制依赖难以快速组合的系统能力；这是加分证据，但不自动等于创新。

---

## 4. 新增 Default Workflow Test

真正强的产品机制不只是“可以这样用”，而是试图让它成为默认工作方式。

检查：

1. 用户是否持续使用该机制，而非偶尔调用 AI？
2. 机制是否位于主工作流，而非边缘功能？
3. 是否替代/删除旧步骤，而不只是增加一步 AI？
4. 是否形成持久对象或状态？
5. 多次使用后，产品是否因 Context / Evidence / State 积累而变得不同？

如果多数为否，最高通常为 Observe。

---

## 5. 新增 Responsibility Shift Test

明确写出：

| 角色 | Before | After |
|---|---|---|
| Human | 做什么 | 还做什么 / 不再做什么 |
| AI | 无 / 辅助 | 新承担什么责任 |
| Deterministic Software | 执行规则 | 新承担什么确定性边界 |

关键不是“AI 做更多”，而是：

> **谁负责发起？谁负责判断？谁拥有状态？谁拥有权限？谁承担失败？谁决定完成？**

只有责任边界发生实质变化，才属于强机制信号。

---

## 6. Pattern 不再“发现即编号”

这是 v2 最大治理变化。

新机制必须经历：

```text
Observation
→ Mechanism Candidate
→ Repeated Evidence
→ Pattern
→ Mature Pattern
→ Merge / Split / Deprecate
```

### 新增 Pattern 的最低条件

满足以下之一：

- 至少 **3 个相互独立产品**出现同一结构性机制；或
- 1 个极强产品案例，但 Mechanism Delta 极其清晰，且无法被现有 Pattern 表达。

同时必须完成：

1. 与全部现有 Pattern 做 nearest-neighbor 比较。
2. 写清楚“现有 Pattern 为什么解释不了”。
3. 给出反例/边界。
4. 定义该 Pattern 的必要条件，而不只是代表产品。
5. 判断它是产品模式、架构原则还是实现技术。

默认先进入 **Candidate**，不立即占用新编号。

### Pattern 也允许死亡

每月检查：
- Merge：两个 Pattern 本质相同。
- Split：一个 Pattern 定义过宽。
- Deprecate：只是阶段性实现方式。
- Promote：Candidate 得到足够独立证据。

编号不是荣誉，不追求只增不减。

---

## 7. 评分从一个数字拆成两个维度

仍保留 1–5 的 Tracking Score，方便排序，但不再让它承担全部含义。

每个候选同时记录：

### Mechanism Novelty
- 0：普通 AI feature
- 1：已有模式的新实现
- 2：已有模式的重要扩展
- 3：可能存在新的结构性机制
- 4：明确新机制，已有较强证据

### Evidence Maturity
- A：产品已真实可用 + 多源证据
- B：官方产品/Docs/Demo 清楚，但实际使用证据有限
- C：发布/演示阶段
- D：概念/推断为主

例如：

```text
Paperclip
Tracking: 4/5
Mechanism Novelty: 3
Evidence: B
Status: Pattern Candidate
```

这样不会把“很新但证据弱”和“很成熟但机制普通”混成同一个 4/5。

---

## 8. Finding 成为最小研究单位

以后每天不是简单“发现 Product”，而是产生 Finding：

```yaml
finding:
  date:
  product:
  claim:
  mechanism_delta:
  evidence:
  evidence_type:
  confidence:
  topics:
  supports_patterns:
  challenges_patterns:
  status: confirmed | inferred | unknown | judgment
```

Product 是 Findings 的长期聚合；Daily 是 Findings 的时间视图；Pattern 是多个 Findings 的抽象。

---

## 9. 每日 Radar v2 流程

```text
Discovery
↓
Deduplicate against Products / Findings
↓
Mechanism Delta
↓
Composable Test v2
↓
Default Workflow Test
↓
Responsibility Shift Test
↓
Evidence Maturity
↓
Compare existing Patterns
↓
Core / Observe / Reject
↓
Write Findings
↓
Update Product
↓
Pattern Candidate / Support / Challenge
↓
Generate Daily
↓
Methodology Reflection
```

### Core 的新门槛

进入 Core 必须同时满足：

- Mechanism Delta 清晰；
- 至少一个关键责任/对象/交互/状态发生结构性变化；
- 不是纯能力升级；
- Default Workflow 有成立可能；
- Composable Test 后仍保留结构性差异；
- 有足够证据支持，不只是宣传文案。

---

## 10. 每日日报新增 Methodology Reflection

日报最后检查：

- 今天是否出现现有 Pattern 无法解释的机制？
- 是否出现 Composable Test 边界案例？
- 是否发现某 Pattern 定义过宽/过窄？
- 是否有历史判断被新证据挑战？
- 是否出现新的第一等对象？
- 是否把“基础设施成熟”误判成“产品机制创新”？
- 是否把“新颖 Demo”误判成“默认工作方式”？

只能生成 **Methodology Proposal**，每日 Agent 不自动修改 L1 原则。

---

## 11. 本轮对现有 30 个 Pattern 的复盘判断

暂不直接删除或重编号，避免破坏历史引用，但从今天开始视为 **v1 Pattern Set**。

优先审计以下重叠簇：

- #03 Goal → Autonomous Work / #06 Persistent State → Proactive Action / #28 Generation as State Transition
- #14 Governance Layer / #15 Proposed Action / #22 Agent Identity
- #17 Judgment Queue / #18 Spatial Supervision
- #21 Shared Artifact / #27 Context Belongs to Work
- #04 Organization Context / #19 Organization Learning / #25 Business Semantic Layer
- #09 Generative Interface / #26 Agent-facing System of Record / #29 Operational Model

下一步不是继续增加 #31，而是先检查这些模式之间究竟是：
**父子关系、组合关系、生命周期不同阶段，还是重复命名。**

---

## 12. v2 最核心的一句话

> **不要问“这个 AI 产品有什么新功能”，先问“AI 让哪个原本不存在的一等对象、责任关系或默认工作方式变得成立？”**

如果答案只是“Agent 自动做了更多步骤”，默认不是强产品机制创新。

---

## 13. v2.2 执行完整性：Delivery Verification（2026-10-08）

对 GitHub 仓库、Pages 或自动化逻辑的任何修改，执行 **read → change → commit → read-back**：

1. 修改前读取 main 的实际文件，不根据历史日报推断当前代码状态。
2. 提交后回读 main 目标文件/commit，确认修改确实存在。
3. 如果无法访问已部署 Pages，明确区分“仓库源码已提交”和“线上页面已验证”，禁止混用。
4. 日报里只报告已经证实的动作；失败或未完成的工作明确记录，不得以完成式书写。
5. 评分时区分 Mechanism Novelty 与 Distribution/Default Adoption；已有机制获得大规模默认使用，不自动产生新 Pattern。

该条是低风险交付质量规则，不改变 L1 创新定义。
