# Skill Fusion: gstack + superpowers

> gstack 负责"想"，superpowers 负责"做"。决策层和执行层各取最强，剩下的全砍掉。

[English](#english) | [中文](#中文)

---

## 中文

### 问题

[gstack](https://github.com/garrytan/gstack) 和 [superpowers](https://github.com/obra/superpowers-marketplace) 是目前 Claude Code 生态中最流行的两套 skill 框架。很多人同时安装了两者，但它们有 **7 个能力重叠**，会在同一个会话中争夺控制权：

| 能力 | gstack | superpowers |
|---|---|---|
| 头脑风暴 | `/office-hours` | `brainstorming` |
| 计划 | `/plan-*-review` (x3) | `writing-plans` |
| 调试 | `/investigate` | `systematic-debugging` |
| 代码审查 | `/review` | `requesting-code-review` |
| 发布 | `/ship` | `finishing-a-development-branch` |

默认行为：两套 skill 全量加载，模型随机选择，结果不可预测。

### 方案

**硬路由，不靠模型自觉。**

分析两者各自的强项后，我们发现一个清晰的分工模式：

- **gstack 强在决策层**：多视角计划审查（CEO/Eng/Design）、交互式头脑风暴、结构化调试、安全审计
- **superpowers 强在执行层**：TDD 纪律、并行代理分发、计划执行检查点、完成前验证

融合策略：**gstack 包裹两端（决策 + 审查），superpowers 负责中间（执行纪律）**。重叠能力只走指定的一方，另一方的对应 skill 被显式禁止。

### 路由表

| 阶段 | 用什么 | 具体 skill | 禁止触发 |
|---|---|---|---|
| 头脑风暴 | gstack | `/office-hours` | `superpowers:brainstorming` |
| 计划策略 | gstack | `/plan-ceo-review` | `superpowers:writing-plans` |
| 计划架构 | gstack | `/plan-eng-review` | `superpowers:writing-plans` |
| 计划设计 | gstack | `/plan-design-review` | `superpowers:writing-plans` |
| 调试 | gstack | `/investigate` | `superpowers:systematic-debugging` |
| 代码审查 | gstack | `/review` | `superpowers:requesting-code-review` |
| 发布/PR | gstack | `/ship` | `superpowers:finishing-a-development-branch` |
| 计划执行 | superpowers | `executing-plans` | — |
| TDD | superpowers | `test-driven-development` | — |
| 并行分发 | superpowers | `dispatching-parallel-agents` | — |
| 完成前验证 | superpowers | `verification-before-completion` | — |
| Git worktree | superpowers | `using-git-worktrees` | — |
| 子代理协调 | superpowers | `subagent-driven-development` | — |
| 处理审查反馈 | superpowers | `receiving-code-review` | — |

### 工作流管线

```
想法 → gstack /office-hours         (头脑风暴)
     → gstack /plan-ceo-review      (策略审查)
     → gstack /plan-eng-review      (架构审查)
     → gstack /plan-design-review   (设计审查)
     → superpowers executing-plans   (带检查点执行)
       ├─ superpowers TDD           (测试驱动实现)
       ├─ superpowers parallel-agents(并行子任务)
       └─ superpowers verification   (完成前验证)
     → gstack /review               (代码审查)
     → gstack /ship                 (发布)
     → gstack /qa                   (上线后 QA)
     → gstack /canary               (金丝雀监控)
```

不是每个任务都要走完整管线：
- **小任务**（<3 步）：直接执行，跳过头脑风暴和计划审查
- **中任务**（3-7 步）：从 `/plan-eng-review` 开始
- **大任务**（>7 步）：走完整管线

### 安装

将 [`claude-md-snippet.md`](./claude-md-snippet.md) 的内容复制到你的 `~/.claude/CLAUDE.md` 中。

或者直接告诉 Claude Code：

```
读取 /path/to/skill-fusion/claude-md-snippet.md，
把内容追加到我的 ~/.claude/CLAUDE.md 末尾
```

### 前提

- 已安装 [gstack](https://github.com/garrytan/gstack)（`~/.claude/skills/gstack/`）
- 已启用 [superpowers](https://github.com/obra/superpowers-marketplace)（`settings.json` 中 `superpowers@superpowers-marketplace: true`）

---

## English

### Problem

[gstack](https://github.com/garrytan/gstack) and [superpowers](https://github.com/obra/superpowers-marketplace) are the two most popular skill frameworks in the Claude Code ecosystem. Many users install both, but they have **7 overlapping capabilities** that compete for control in the same session.

Default behavior: both skill sets load fully, the model picks randomly, results are unpredictable.

### Solution

**Hard routing, not model discretion.**

After analyzing each framework's strengths:

- **gstack excels at the decision layer**: multi-perspective plan review (CEO/Eng/Design), interactive brainstorming, structured debugging, security audits
- **superpowers excels at the execution layer**: TDD discipline, parallel agent dispatch, plan execution checkpoints, pre-completion verification

Fusion strategy: **gstack wraps both ends (decision + review), superpowers handles the middle (execution discipline)**. Overlapping capabilities are routed to exactly one framework; the other's corresponding skill is explicitly suppressed.

### Routing Table

| Phase | Framework | Skill | Suppressed |
|---|---|---|---|
| Brainstorming | gstack | `/office-hours` | `superpowers:brainstorming` |
| Plan (strategy) | gstack | `/plan-ceo-review` | `superpowers:writing-plans` |
| Plan (architecture) | gstack | `/plan-eng-review` | `superpowers:writing-plans` |
| Plan (design) | gstack | `/plan-design-review` | `superpowers:writing-plans` |
| Debugging | gstack | `/investigate` | `superpowers:systematic-debugging` |
| Code review | gstack | `/review` | `superpowers:requesting-code-review` |
| Ship / PR | gstack | `/ship` | `superpowers:finishing-a-development-branch` |
| Plan execution | superpowers | `executing-plans` | — |
| TDD | superpowers | `test-driven-development` | — |
| Parallel dispatch | superpowers | `dispatching-parallel-agents` | — |
| Pre-completion check | superpowers | `verification-before-completion` | — |
| Git worktrees | superpowers | `using-git-worktrees` | — |
| Subagent coordination | superpowers | `subagent-driven-development` | — |
| Handling review feedback | superpowers | `receiving-code-review` | — |

### Workflow Pipeline

```
Idea → gstack /office-hours         (brainstorm)
     → gstack /plan-ceo-review      (strategy review)
     → gstack /plan-eng-review      (architecture review)
     → gstack /plan-design-review   (design review)
     → superpowers executing-plans   (execute with checkpoints)
       ├─ superpowers TDD           (test-driven implementation)
       ├─ superpowers parallel-agents(parallel subtasks)
       └─ superpowers verification   (pre-completion verification)
     → gstack /review               (code review)
     → gstack /ship                 (ship)
     → gstack /qa                   (post-ship QA)
     → gstack /canary               (canary monitoring)
```

Not every task needs the full pipeline:
- **Small tasks** (<3 steps): Execute directly, skip brainstorming and plan review
- **Medium tasks** (3-7 steps): Start from `/plan-eng-review`
- **Large tasks** (>7 steps): Full pipeline

### Installation

Copy the contents of [`claude-md-snippet.md`](./claude-md-snippet.md) into your `~/.claude/CLAUDE.md`.

### Prerequisites

- [gstack](https://github.com/garrytan/gstack) installed at `~/.claude/skills/gstack/`
- [superpowers](https://github.com/obra/superpowers-marketplace) enabled in `settings.json`

---

## License

MIT
