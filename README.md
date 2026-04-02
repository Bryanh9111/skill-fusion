# Skill Fusion: gstack + superpowers + octopus + token-efficient

> 四层架构，不是四套工具。gstack 做骨架，superpowers 做纪律，octopus 做升级，token-efficient 做压缩。

[English](#english) | [中文](#中文)

---

## 中文

### 问题

[gstack](https://github.com/garrytan/gstack)、[superpowers](https://github.com/obra/superpowers-marketplace)、[claude-octopus](https://github.com/nyldn/claude-octopus) 和 [claude-token-efficient](https://github.com/drona23/claude-token-efficient) 是 Claude Code 生态中最强的几套增强框架。同时安装后，前三套在 **11 个能力域** 存在重叠，会在同一个会话中争夺控制权；而 token-efficient 的规则如果不与现有 CLAUDE.md 整合，会产生冗余或冲突。

默认行为：全量加载，模型随机选择，结果不可预测，高成本 skill 可能被无谓调用，输出冗长浪费 token。

### 方案

**统一控制平面：四层架构 + 意图风险路由 + 三级 escalation。**

分析各自强项后，我们找到了清晰的分层模式：

- **gstack** = 默认路由与工作流骨架（决策入口 + 审查 + 交付）
- **superpowers** = 全局纪律与执行策略层（行为约束，不是工具）
- **octopus** = 升级层与专项能力（多模型共识 / 辩论 / 深度研究 / 文档产出）
- **token-efficient** = 输出压缩层（全局被动约束，减少约 63% 冗余 token）

核心机制：**默认单路快跑 → 失败后升级 → 分歧大时进入对抗 → 所有输出经过 token 压缩。** 硬路由，不靠模型自觉。

### token-efficient 融合策略

[claude-token-efficient](https://github.com/drona23/claude-token-efficient) 不是 skill 框架，不参与路由或 escalation。它是一组**输出行为约束**，通过合并到 CLAUDE.md 的现有段落来生效，而不是作为独立文件叠加。

**为什么合并而不是叠加？**
- 独立 CLAUDE.md 文件会增加每条消息的 input token（文件本身约 500-1000 token）
- 它的 8 条核心规则与现有 CLAUDE.md 高度重叠（sycophancy 禁令、terse output、read-before-write 等）
- 合并后消除冗余，净 token 节约更高

**融合映射 — 8 条规则的去向：**

| token-efficient 规则 | 融入 CLAUDE.md 段落 | 说明 |
|---|---|---|
| Think first, read before writing | `<context_gathering>` | 已有"Prefer acting over more searching"，补充"read files before editing" |
| Keep output concise | Communication: "stay terse" | 已有，原文保留 |
| Prefer targeted edits over full rewrites | Code Editing Rules | 已有"favor simple, modular solutions" |
| Read each file once unless changed | Communication: "Don't re-read files" | 已有，原文保留 |
| Run tests before finishing | `<self_reflection>` + verification step | 已有 ≥90% coverage 要求 |
| No flattering preamble/closing fluff | Communication: "No sycophancy" | 已有，原文保留 |
| Favor simple direct fixes | Priority #1: KISS/YAGNI | 已有，核心原则 |
| User instructions always override | Priority stack rule #1 | 已有，最高优先级 |

**结论：** 8 条规则中 **0 条需要新增**，全部已被现有 CLAUDE.md 覆盖。token-efficient 的价值在于验证了这些规则的有效性（benchmark 显示 63% token 减少），而非引入新规则。

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
- [claude-token-efficient](https://github.com/drona23/claude-token-efficient) 的规则已合并到 CLAUDE.md（不需要单独安装）

### 相关框架

先安装以下框架，再用本 repo 的融合规则进行整合：

| 框架 | 仓库 | 安装方式 | 角色 |
|---|---|---|---|
| **gstack** | [garrytan/gstack](https://github.com/garrytan/gstack) | Claude Code skill | 工作流骨架 |
| **superpowers** | [obra/superpowers-marketplace](https://github.com/obra/superpowers-marketplace) | Claude Code plugin | 执行纪律 |
| **claude-octopus** | [nyldn/claude-octopus](https://github.com/nyldn/claude-octopus) | Claude Code plugin | 多 AI 升级 |
| **claude-token-efficient** | [drona23/claude-token-efficient](https://github.com/drona23/claude-token-efficient) | 合并到 CLAUDE.md | 输出压缩 |

---

## English

### Problem

[gstack](https://github.com/garrytan/gstack), [superpowers](https://github.com/obra/superpowers-marketplace), [claude-octopus](https://github.com/nyldn/claude-octopus), and [claude-token-efficient](https://github.com/drona23/claude-token-efficient) are among the most powerful enhancement frameworks in the Claude Code ecosystem. When installed together, the first three have **11 overlapping capability domains** that compete for control; token-efficient's rules can create redundancy if not merged with existing CLAUDE.md constraints.

Default behavior: all skill sets load fully, the model picks randomly, results are unpredictable, expensive multi-AI skills may fire unnecessarily, and verbose output wastes tokens.

### Solution

**Unified control plane: four-layer architecture + intent/risk routing + three-level escalation.**

After analyzing each framework's strengths:

- **gstack** = default router and workflow backbone (decision entry + review + delivery)
- **superpowers** = global discipline and execution policy layer (behavioral constraints, not tools)
- **octopus** = escalation layer and specialty engine (multi-model consensus / debate / deep research / document generation)
- **token-efficient** = output compression layer (passive global constraint, ~63% token reduction)

Core mechanism: **fast single-model default → escalate on failure → adversarial debate on high disagreement → all output compressed.** Hard routing, not model discretion.

### token-efficient Integration Strategy

[claude-token-efficient](https://github.com/drona23/claude-token-efficient) is not a skill framework -- it does not participate in routing or escalation. It is a set of **output behavioral constraints** that are merged into existing CLAUDE.md sections rather than added as a separate file.

**Why merge instead of stack?**
- A standalone CLAUDE.md file adds ~500-1000 input tokens per message
- Its 8 core rules overlap heavily with existing CLAUDE.md (sycophancy ban, terse output, read-before-write, etc.)
- Merging eliminates redundancy and maximizes net token savings

**Fusion mapping -- where the 8 rules land:**

| token-efficient rule | Merged into CLAUDE.md section | Status |
|---|---|---|
| Think first, read before writing | `<context_gathering>` | Already covered |
| Keep output concise | Communication: "stay terse" | Already covered |
| Prefer targeted edits over full rewrites | Code Editing Rules | Already covered |
| Read each file once unless changed | Communication: "Don't re-read files" | Already covered |
| Run tests before finishing | `<self_reflection>` + verification step | Already covered |
| No flattering preamble/closing fluff | Communication: "No sycophancy" | Already covered |
| Favor simple direct fixes | Priority #1: KISS/YAGNI | Already covered |
| User instructions always override | Priority stack rule #1 | Already covered |

**Conclusion:** All 8 rules are already covered by the existing CLAUDE.md. token-efficient's value is validating these rules work (benchmarked at 63% token reduction), not introducing new ones.

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
- [claude-token-efficient](https://github.com/drona23/claude-token-efficient) rules merged into CLAUDE.md (no separate install needed)

---

## Referenced Frameworks

Install these first, then use this repo's fusion rules to integrate them:

| Framework | Repo | Install Method | Role |
|---|---|---|---|
| **gstack** | [garrytan/gstack](https://github.com/garrytan/gstack) | Claude Code skill | Workflow backbone |
| **superpowers** | [obra/superpowers-marketplace](https://github.com/obra/superpowers-marketplace) | Claude Code plugin | Execution discipline |
| **claude-octopus** | [nyldn/claude-octopus](https://github.com/nyldn/claude-octopus) | Claude Code plugin | Multi-AI escalation |
| **claude-token-efficient** | [drona23/claude-token-efficient](https://github.com/drona23/claude-token-efficient) | Merge into CLAUDE.md | Output compression |

---

## License

MIT
