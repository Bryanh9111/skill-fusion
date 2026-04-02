# Skill Fusion: gstack + superpowers + octopus

> 三套框架不是平级并存，而是分层协作。gstack 做骨架，superpowers 做纪律，octopus 做升级。

[English](#english) | [中文](#中文)

---

## 中文

### 问题

[gstack](https://github.com/garrytan/gstack)、[superpowers](https://github.com/obra/superpowers-marketplace) 和 [claude-octopus](https://github.com/nyldn/claude-octopus) 是 Claude Code 生态中最强的三套 skill 框架。同时安装后，它们在 **11 个能力域** 存在重叠，会在同一个会话中争夺控制权。

默认行为：三套 skill 全量加载，模型随机选择，结果不可预测，高成本 skill 可能被无谓调用。

### 方案

**统一控制平面：意图 + 风险路由，三级 escalation。**

分析三者各自的强项后，我们找到了清晰的分层模式：

- **gstack** = 默认路由与工作流骨架（决策入口 + 审查 + 交付）
- **superpowers** = 全局纪律与执行策略层（行为约束，不是工具）
- **octopus** = 升级层与专项能力（多模型共识 / 辩论 / 深度研究 / 文档产出）

核心机制：**默认单路快跑 → 失败后升级 → 分歧大时进入对抗。** 硬路由，不靠模型自觉。

### 三级 Escalation 架构

```
L1 (cheap)    → gstack / superpowers 单模型，快速低成本
L2 (standard) → octopus 多模型共识，质量门控触发
L3 (premium)  → octopus debate/embrace，对抗性辩论
```

高风险意图（architecture + high、security + high）可跳过 L1 直接进 L2。

### 六大主域路由

#### 域 A：构思与规划

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/office-hours`、`/plan-ceo-review`、`/plan-eng-review`、`/plan-design-review` |
| L2 | octopus | `/octo:brainstorm`、`/octo:plan`、`/octo:design-ui-ux` |
| L3 | octopus | `/octo:debate`、`/octo:prd` |

#### 域 B：实现与执行

| 级别 | 执行者 | Skill |
|---|---|---|
| 默认 | superpowers | `executing-plans`、`test-driven-development`、`subagent-driven-development` |
| 大规模并行 | superpowers | `dispatching-parallel-agents` |
| 复杂多团队 | octopus | `/octo:parallel` |
| 自治流水线 | octopus | `/octo:factory`（独占） |

#### 域 C：审查与验证

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/review`、`/qa` |
| L2 | octopus | `/octo:review`、`/octo:deliver` |
| L3 | octopus | `/octo:staged-review`、`/octo:debate` |

#### 域 D：调试与安全

| 级别 | 执行者 | Skill |
|---|---|---|
| L1 | gstack | `/investigate`、`/cso` |
| L2 | octopus | `/octo:debug`、`/octo:security` |
| L3 | octopus | `/octo:debate` |

#### 域 E：交付与发布

| 执行者 | Skill |
|---|---|
| gstack 独占 | `/ship`、`/land-and-deploy`、`/canary`、`/benchmark`、`/browse` |

不做自动 escalation。

#### 域 F：知识沉淀与文档

| 子域 | 执行者 | Skill |
|---|---|---|
| 回顾 | gstack L1 → octopus L2 | `/retro` → `/octo:retro` |
| 发布后文档 | gstack 独占 | `/document-release` |
| 深度研究 | octopus 独占 | `/octo:research` |
| 文档/演示/规格 | octopus 独占 | `/octo:docs`、`/octo:deck`、`/octo:spec` |

### 升级门控（机器可判定）

**L1 → L2 触发条件（任一满足）：**
- blocker 缺陷 ≥ 1
- high severity 缺陷 ≥ 3
- 总 issue 数 ≥ 7
- 低置信度输出（"不确定根因"、"可能有多个解释"等）
- 跨边界改动（auth / payments / deploy / infra / schema / shared library）
- 改动文件数 > 10
- 用户要求多视角

**L2 → L3 触发条件（任一满足）：**
- Provider 共识率 < 70%
- Top-3 recommendations 有明显矛盾
- 高代价不可逆决策（架构范式、数据迁移、安全策略）
- 用户对抗性指令（"escalate"、"辩论"、"不服"）

### 反升级机制（Budget Guard）

| 规则 | 说明 |
|---|---|
| 单次最多升一级 | 普通任务 L1→L2 即止，除非 high risk 或用户明确要求 L3 |
| 同一工件冷却期 | 同一 PR/plan 已过一次 octo 处理，30min 内不再自动重复 |
| 成本标签 | cheap → standard → premium，默认只跑 cheap |
| release 域禁止升级 | ship / deploy / canary / benchmark 不做自动 escalation |

### 全局纪律层（superpowers as policy）

以下始终生效，不是可选 skill：

- `verification-before-completion` — 完成前必须验证
- `executing-plans` — 多步计划带检查点
- `test-driven-development` — 先测试后实现
- `using-git-worktrees` — 特性开发用隔离 worktree
- `receiving-code-review` — 审查反馈先验证再改
- `subagent-driven-development` — 独立任务用子代理并行

### 完整工作流管线

```
想法 → gstack /office-hours              (L1 构思)
       ↳ 不满意? /octo:brainstorm        (L2 多 AI 创意)
       ↳ 分歧大? /octo:debate            (L3 方案对决)
     → gstack /plan-ceo-review            (L1 策略)
       ↳ high risk? /octo:plan            (L2 多 AI 规划)
       ↳ 需要评分? /octo:prd             (L3 PRD 评分)
     → gstack /plan-eng-review            (L1 架构)
       ↳ 架构分歧? /octo:debate          (L3 架构辩论)
     → gstack /plan-design-review         (L1 设计)
       ↳ 需要多 AI? /octo:design-ui-ux   (L2 BM25 设计)
     → superpowers executing-plans         (带检查点执行)
       ├─ superpowers TDD                 (测试驱动实现)
       ├─ superpowers parallel-agents     (并行子任务)
       │  ↳ 超大规模? /octo:parallel      (Team of Teams)
       └─ superpowers verification         (完成前验证)
     → gstack /review                      (L1 代码审查)
       ↳ 门控触发? /octo:review           (L2 多 LLM 审查)
       ↳ 分歧大? /octo:debate             (L3 对抗审查)
     → gstack /ship                        (发布)
     → gstack /qa                          (上线后 QA)
       ↳ 门控触发? /octo:deliver          (L2 多 AI 验收)
     → gstack /canary                      (金丝雀监控)
```

管线入口规则：
- **小任务**（<3 步，low risk）：直接执行，只走 L1
- **中任务**（3-7 步，medium risk）：从 `/plan-eng-review` 开始，允许自动 L2
- **大任务**（>7 步，high risk）：走完整管线，允许直接 L2/L3

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
- 已安装 [claude-octopus](https://github.com/nyldn/claude-octopus)（`claude plugin install octo@nyldn-plugins`）

---

## English

### Problem

[gstack](https://github.com/garrytan/gstack), [superpowers](https://github.com/obra/superpowers-marketplace), and [claude-octopus](https://github.com/nyldn/claude-octopus) are the three most powerful skill frameworks in the Claude Code ecosystem. When installed together, they have **11 overlapping capability domains** that compete for control in the same session.

Default behavior: all three skill sets load fully, the model picks randomly, results are unpredictable, and expensive multi-AI skills may fire unnecessarily.

### Solution

**Unified control plane: intent + risk routing with three-level escalation.**

After analyzing each framework's strengths:

- **gstack** = default router and workflow backbone (decision entry + review + delivery)
- **superpowers** = global discipline and execution policy layer (behavioral constraints, not tools)
- **octopus** = escalation layer and specialty engine (multi-model consensus / debate / deep research / document generation)

Core mechanism: **fast single-model default → escalate on failure → adversarial debate on high disagreement.** Hard routing, not model discretion.

### Three-Level Escalation Architecture

```
L1 (cheap)    → gstack / superpowers single-model, fast and low-cost
L2 (standard) → octopus multi-model consensus, triggered by quality gates
L3 (premium)  → octopus debate/embrace, adversarial deliberation
```

High-risk intents (architecture + high, security + high) may skip L1 and enter L2 directly.

### Six Routing Domains

#### Domain A: Ideation & Planning

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review` |
| L2 | octopus | `/octo:brainstorm`, `/octo:plan`, `/octo:design-ui-ux` |
| L3 | octopus | `/octo:debate`, `/octo:prd` |

#### Domain B: Implementation & Execution

| Level | Owner | Skills |
|---|---|---|
| Default | superpowers | `executing-plans`, `test-driven-development`, `subagent-driven-development` |
| Large-scale parallel | superpowers | `dispatching-parallel-agents` |
| Complex multi-team | octopus | `/octo:parallel` |
| Autonomous pipeline | octopus | `/octo:factory` (exclusive) |

#### Domain C: Review & Validation

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/review`, `/qa` |
| L2 | octopus | `/octo:review`, `/octo:deliver` |
| L3 | octopus | `/octo:staged-review`, `/octo:debate` |

#### Domain D: Debugging & Security

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/investigate`, `/cso` |
| L2 | octopus | `/octo:debug`, `/octo:security` |
| L3 | octopus | `/octo:debate` |

#### Domain E: Delivery & Release

| Owner | Skills |
|---|---|
| gstack exclusive | `/ship`, `/land-and-deploy`, `/canary`, `/benchmark`, `/browse` |

No automatic escalation.

#### Domain F: Knowledge & Documentation

| Subdomain | Owner | Skills |
|---|---|---|
| Retrospective | gstack L1 → octopus L2 | `/retro` → `/octo:retro` |
| Post-release docs | gstack exclusive | `/document-release` |
| Deep research | octopus exclusive | `/octo:research` |
| Docs/deck/spec | octopus exclusive | `/octo:docs`, `/octo:deck`, `/octo:spec` |

### Escalation Gates (Machine-Judgeable)

**L1 → L2 triggers (any one):**
- Blocker defects ≥ 1
- High severity defects ≥ 3
- Total issues ≥ 7
- Low-confidence output ("uncertain root cause", "multiple possible explanations")
- Cross-boundary changes (auth / payments / deploy / infra / schema / shared library)
- Changed files > 10
- User requests multiple perspectives

**L2 → L3 triggers (any one):**
- Provider consensus < 70%
- Top-3 recommendations conflict
- High-cost irreversible decisions (architecture paradigm, data migration, security policy)
- User adversarial instructions ("escalate", "debate", "challenge this")

### Budget Guard (Anti-Escalation)

| Rule | Description |
|---|---|
| One level per escalation | Normal tasks stop at L2; L3 requires high risk or explicit request |
| Cooldown period | Same artifact processed by octo: 30min cooldown before auto-retry |
| Cost labels | cheap → standard → premium; default is cheap only |
| Release domain locked | ship / deploy / canary / benchmark: no auto-escalation |

### Global Discipline Layer (superpowers as policy)

Always active, not optional skills:

- `verification-before-completion` — must verify before claiming done
- `executing-plans` — multi-step plans with checkpoints
- `test-driven-development` — tests before implementation
- `using-git-worktrees` — isolated worktrees for feature work
- `receiving-code-review` — verify feedback before implementing
- `subagent-driven-development` — parallel subagents for independent tasks

### Full Workflow Pipeline

```
Idea → gstack /office-hours              (L1 brainstorm)
       ↳ not satisfied? /octo:brainstorm  (L2 multi-AI creativity)
       ↳ high disagreement? /octo:debate  (L3 adversarial)
     → gstack /plan-ceo-review            (L1 strategy)
       ↳ high risk? /octo:plan            (L2 multi-AI planning)
       ↳ needs scoring? /octo:prd         (L3 PRD scoring)
     → gstack /plan-eng-review            (L1 architecture)
       ↳ arch disagreement? /octo:debate  (L3 architecture debate)
     → gstack /plan-design-review         (L1 design)
       ↳ needs multi-AI? /octo:design     (L2 BM25 design)
     → superpowers executing-plans         (execute with checkpoints)
       ├─ superpowers TDD                 (test-driven implementation)
       ├─ superpowers parallel-agents     (parallel subtasks)
       │  ↳ massive scale? /octo:parallel  (Team of Teams)
       └─ superpowers verification         (pre-completion verification)
     → gstack /review                      (L1 code review)
       ↳ gate triggered? /octo:review     (L2 multi-LLM review)
       ↳ high disagreement? /octo:debate  (L3 adversarial review)
     → gstack /ship                        (ship)
     → gstack /qa                          (post-ship QA)
       ↳ gate triggered? /octo:deliver    (L2 multi-AI acceptance)
     → gstack /canary                      (canary monitoring)
```

Pipeline entry rules:
- **Small tasks** (<3 steps, low risk): Execute directly, L1 only
- **Medium tasks** (3-7 steps, medium risk): Start from `/plan-eng-review`, allow auto-L2
- **Large tasks** (>7 steps, high risk): Full pipeline, allow direct L2/L3

### Installation

Copy the contents of [`claude-md-snippet.md`](./claude-md-snippet.md) into your `~/.claude/CLAUDE.md`.

### Prerequisites

- [gstack](https://github.com/garrytan/gstack) installed at `~/.claude/skills/gstack/`
- [superpowers](https://github.com/obra/superpowers-marketplace) enabled in `settings.json`
- [claude-octopus](https://github.com/nyldn/claude-octopus) installed via `claude plugin install octo@nyldn-plugins`

---

## License

MIT
