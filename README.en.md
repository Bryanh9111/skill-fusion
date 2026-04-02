# Skill Fusion: gstack + superpowers + octopus + token-efficient

> Four layers, not four tools. gstack for backbone, superpowers for discipline, octopus for escalation, token-efficient for compression.

**[中文](./README.md)**

## Problem

[gstack](https://github.com/garrytan/gstack), [superpowers](https://github.com/obra/superpowers-marketplace), [claude-octopus](https://github.com/nyldn/claude-octopus), and [claude-token-efficient](https://github.com/drona23/claude-token-efficient) are among the most powerful enhancement frameworks in the Claude Code ecosystem. When installed together, the first three have **11 overlapping capability domains** that compete for control; token-efficient's rules can create redundancy if not merged with existing CLAUDE.md constraints.

Default behavior: all skill sets load fully, the model picks randomly, results are unpredictable, expensive multi-AI skills may fire unnecessarily, and verbose output wastes tokens.

## Solution

**Unified control plane: four-layer architecture + intent/risk routing + three-level escalation.**

After analyzing each framework's strengths:

- **gstack** = default router and workflow backbone (decision entry + review + delivery)
- **superpowers** = global discipline and execution policy layer (behavioral constraints, not tools)
- **octopus** = escalation layer and specialty engine (multi-model consensus / debate / deep research / document generation)
- **token-efficient** = output compression layer (passive global constraint, ~63% token reduction)

Core mechanism: **fast single-model default → escalate on failure → adversarial debate on high disagreement → all output compressed.** Hard routing, not model discretion.

## token-efficient Integration Strategy

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

## Three-Level Escalation Architecture

```
L1 (cheap)    → gstack / superpowers single-model, fast and low-cost
L2 (standard) → octopus multi-model consensus, triggered by quality gates
L3 (premium)  → octopus debate/embrace, adversarial deliberation
```

High-risk intents (architecture + high, security + high) may skip L1 and enter L2 directly.

## Six Routing Domains

### Domain A: Ideation & Planning

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review` |
| L2 | octopus | `/octo:brainstorm`, `/octo:plan`, `/octo:design-ui-ux` |
| L3 | octopus | `/octo:debate`, `/octo:prd` |

### Domain B: Implementation & Execution

| Level | Owner | Skills |
|---|---|---|
| Default | superpowers | `executing-plans`, `test-driven-development`, `subagent-driven-development` |
| Large-scale parallel | superpowers | `dispatching-parallel-agents` |
| Complex multi-team | octopus | `/octo:parallel` |
| Autonomous pipeline | octopus | `/octo:factory` (exclusive) |

### Domain C: Review & Validation

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/review`, `/qa` |
| L2 | octopus | `/octo:review`, `/octo:deliver` |
| L3 | octopus | `/octo:staged-review`, `/octo:debate` |

### Domain D: Debugging & Security

| Level | Owner | Skills |
|---|---|---|
| L1 | gstack | `/investigate`, `/cso` |
| L2 | octopus | `/octo:debug`, `/octo:security` |
| L3 | octopus | `/octo:debate` |

### Domain E: Delivery & Release

| Owner | Skills |
|---|---|
| gstack exclusive | `/ship`, `/land-and-deploy`, `/canary`, `/benchmark`, `/browse` |

No automatic escalation.

### Domain F: Knowledge & Documentation

| Subdomain | Owner | Skills |
|---|---|---|
| Retrospective | gstack L1 → octopus L2 | `/retro` → `/octo:retro` |
| Post-release docs | gstack exclusive | `/document-release` |
| Deep research | octopus exclusive | `/octo:research` |
| Docs/deck/spec | octopus exclusive | `/octo:docs`, `/octo:deck`, `/octo:spec` |

## Escalation Gates (Machine-Judgeable)

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

## Budget Guard (Anti-Escalation)

| Rule | Description |
|---|---|
| One level per escalation | Normal tasks stop at L2; L3 requires high risk or explicit request |
| Cooldown period | Same artifact processed by octo: 30min cooldown before auto-retry |
| Cost labels | cheap → standard → premium; default is cheap only |
| Release domain locked | ship / deploy / canary / benchmark: no auto-escalation |

## Global Discipline Layer (superpowers as policy)

Always active, not optional skills:

- `verification-before-completion` -- must verify before claiming done
- `executing-plans` -- multi-step plans with checkpoints
- `test-driven-development` -- tests before implementation
- `using-git-worktrees` -- isolated worktrees for feature work
- `receiving-code-review` -- verify feedback before implementing
- `subagent-driven-development` -- parallel subagents for independent tasks

## Full Workflow Pipeline

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

## Installation

Copy the contents of [`claude-md-snippet.md`](./claude-md-snippet.md) into your `~/.claude/CLAUDE.md`.

## Prerequisites

- [gstack](https://github.com/garrytan/gstack) installed at `~/.claude/skills/gstack/`
- [superpowers](https://github.com/obra/superpowers-marketplace) enabled in `settings.json`
- [claude-octopus](https://github.com/nyldn/claude-octopus) installed via `claude plugin install octo@nyldn-plugins`
- [claude-token-efficient](https://github.com/drona23/claude-token-efficient) rules merged into CLAUDE.md (no separate install needed)

## Referenced Frameworks

Install these first, then use this repo's fusion rules to integrate them:

| Framework | Repo | Install Method | Role |
|---|---|---|---|
| **gstack** | [garrytan/gstack](https://github.com/garrytan/gstack) | Claude Code skill | Workflow backbone |
| **superpowers** | [obra/superpowers-marketplace](https://github.com/obra/superpowers-marketplace) | Claude Code plugin | Execution discipline |
| **claude-octopus** | [nyldn/claude-octopus](https://github.com/nyldn/claude-octopus) | Claude Code plugin | Multi-AI escalation |
| **claude-token-efficient** | [drona23/claude-token-efficient](https://github.com/drona23/claude-token-efficient) | Merge into CLAUDE.md | Output compression |

## License

MIT
