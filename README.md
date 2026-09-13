# AI Product Innovation Radar

持续记录真正具有 **AI 应用层产品机制创新** 的产品，而不是普通 AI 新品列表。

截至 2026-09-13：
- 核心入选：**33**
- 观察名单：**3**
- 主动淘汰/仅供参考：**10**
- 共记录：**46**

## 筛选原则

1. 优先关注改变 Human / AI / Deterministic Software 分工的产品。
2. 强制执行 **Composable Test**：如果用 API / CLI / MCP / Agent / Automation 在 1–2 天可拼出 80% 体验，默认不算强产品机制创新。
3. 商业价值大 ≠ 产品创新强；技术复杂 ≠ 产品创新强。
4. Multi-Agent / MCP / 自动化本身都不算创新。
5. 真正关注：交互原语、工作流、业务对象、Context、持续状态、治理、证据、Human-in-the-loop、Agent Ops。

## 全量索引

| 产品 | 状态 | 创新评分 | 模式 / 机制 | 与你的相关性 |
|---|---|---:|---|---|
| Granola | core | ★★★★★ | Human Signal + Machine Context | 强 |
| Acti | core | ★★★★★ | Existing Surface → AI Action Layer | 中 |
| ChatGPT Work | core | ★★★★★ | Goal → Autonomous Work | 中 |
| Claude Cowork | core | ★★★★☆ | Goal → Autonomous Work | 中 |
| Grok Bot | core | ★★★★☆ | Agent Workspace | 中 |
| TaskShell | watchlist | ★★★☆☆ | Human-Agent Shared Task System | 强 |
| Atlas | core | ★★★★☆ | Organization Context Layer | 中 |
| LapuAI | core | ★★★★☆ | Desktop Tool Use | 中 |
| Contrive | core | ★★★★☆ | Cross-app Command Layer | 中 |
| Airtop Agent Builder | core | ★★★★★ | AI Build → Software Run → AI Repair | 强 |
| Fambot | core | ★★★★☆ | Persistent State → Proactive Action | 中 |
| Fireflies Voice Agents | core | ★★★★☆ | Recorder → Participant → Worker | 中 |
| Perplexity Hybrid Compute | core | ★★★☆☆ | Invisible Compute Routing | 中 |
| Tadata | core | ★★★★★ | Observe → Detect Pattern → Propose Automation | 强 |
| Claudeforce | core | ★★★★☆ | Headless Business System + Generative Interface | 强 |
| Gemini Spark × Google Photos | core | ★★★★☆ | Artifact → Semantic Object → Action | 强 |
| Monid | core | ★★★★★ | Capability-on-Demand | 中 |
| Browzer | core | ★★★★☆ | Source → Self-maintaining Artifact | 强 |
| Nex | core | ★★★★☆ | Deterministic / Probabilistic Split | 强 |
| Airuncode | rejected | ★★☆☆☆ | Existing components productization | 强 |
| Meta Muse | core | ★★★★★ | Agent Workspace + Governance Layer | 强 |
| Relaticle | core | ★★★★☆ | Proposed Action as First-class Object | 强 |
| AIR Security | core | ★★★★☆ | Context Supply Chain | 强 |
| AppGacha | rejected | ★★☆☆☆ | Vibe coding + packaging | 中 |
| Tucky | rejected | ★★☆☆☆ | Notes + Agent + Shortcut | 中 |
| GoodLads | watchlist | ★★★☆☆ | Experiment hypothesis generator | 中 |
| Quincy Hub | core | ★★★★★ | AI Work Queue / Judgment Queue | 强 |
| 49agents IDE | core | ★★★★☆ | Spatial Agent Supervision | 中 |
| WRITER Enterprise Brain | core | ★★★★☆ | Organization Learning Loop | 中 |
| Mastra Factory | rejected | ★★☆☆☆ | Issue → Agent → PR | 强 |
| Feathery Robin | rejected | ★★★☆☆ | AI Build → Deterministic Workflow → Human Review | 中 |
| Typewise Nova | core | ★★★★★ | Agent Operations Agent | 强 |
| Type.com | core | ★★★★☆ | AI Work as Shared Artifact | 中 |
| Thousand | core | ★★★★☆ | Agent as First-class Identity | 强 |
| Speechmark | rejected | ★★☆☆☆ | Local ASR + Notes + MCP | 强 |
| OpenMarket | core | ★★★★★ | Adversarial Agents + Independent Referee | 强 |
| HyperVerge | core | ★★★★☆ | Evidence Gap → Active Investigation | 强 |
| OpenAI Data Agent | core | ★★★★☆ | Business Semantic Layer | 强 |
| Cadenya | watchlist | ★★★☆☆ | Agent runtime infrastructure | 中 |
| Swiftime | rejected | ★★☆☆☆ | Natural language + API | 中 |
| ChatGPT for Financial Services | rejected | ★★★☆☆ | Vertical ChatGPT + deep integrations | 中 |
| Accordio | core | ★★★★★ | Agent-facing System of Record | 强 |
| Switch | core | ★★★★☆ | Context Belongs to Work | 强 |
| Diiverge | core | ★★★★☆ | Generation as State Transition | 中 |
| Harden | rejected | ★★★☆☆ | Tool-call policy enforcement | 强 |
| ThreadRecall / Chat Recall | rejected | ★★★☆☆ | Cross-session memory | 中 |

## 仓库结构

```text
.
├── README.md
├── data/
│   ├── apps.csv
│   └── apps.json
├── daily/YYYY-MM/YYYY-MM-DD.md
├── patterns/patterns.md
└── reports/README.md
```

## 每日更新规则

每日允许 **0–3 个**核心入选；没有达到标准的产品时明确记录“今日无核心入选”。
维护目标不是积累更多产品，而是形成 **AI 原生产品设计模式库**。
