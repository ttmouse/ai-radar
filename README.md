# AI Product Innovation Radar

持续记录真正具有 **AI 应用层产品机制创新** 的产品，而不是普通 AI 新品列表。

> 本仓库从“产品名单”升级为“研究库”：`README.md` 负责索引，`daily/` 保存每日完整判断，`products/` 保存每个产品的持续研究档案，`patterns/` 维护可迁移的 AI 原生产品模式。

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

## 阅读入口

- [产品深度卡片](./products/README.md)
- [AI 原生产品模式库](./patterns/patterns.md)
- [结构化总表 CSV](./data/apps.csv)
- [每日发现](./daily/2026-09/)
- [深度报告](./reports/)

## 仓库结构

```text
.
├── README.md
├── products/                 # 每个产品的详细持续档案
├── daily/YYYY-MM/YYYY-MM-DD.md  # 每日完整分析
├── data/apps.csv             # 结构化全量索引
├── patterns/patterns.md      # 产品机制模式库
└── reports/                  # Full Research 深度报告
```

## 每日更新规则

每日允许 **0–3 个**核心入选；没有达到标准的产品时明确记录“今日无核心入选”。

对每个入选产品必须保留完整分析，不允许只写产品名和一句摘要。至少包含：真实问题、旧/新工作流、核心机制、Composable Test、人/AI/确定性软件分工、可迁移原则、对本地 Agent/B/G 的启发、不确定性与跟踪判断。

维护目标不是积累更多产品，而是形成 **AI 原生产品设计模式库**。